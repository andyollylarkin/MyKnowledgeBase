> Оригинал: https://brandur.org/postgres-connections

Вы начинаете разрабатывать ваш новый проект. Вы слышали хорошие вещи об Postgres, так что вы выбрали эту базу данных. Соответственно рекламе вас удовлетворяет этот инструмент и прогресс идет хорошо. Вы выкладываете свой проект в прод и первое время  как вы надеялись все проходит гладко поскольку Postgres хорошо подходит для производственных решений.

Первые несколько месяцев проходят хорошо, и трафик начинает рости, когда внезапно происходят большой всплеск отказов. Вы копаетесь пытаясь выяснить причину и видите, что ваше приложение получает ошибку при попытке соединения с базой данных. Вы находите этот артифакт в логах:
> FATAL: remaining connection slots are reserved for non-replication superuser connections

Это одна из первых производственных проблем, с которыми сталкиваются новые пользователи Postgres, и это может быть раздражающим. Соответственно описанию ошибки БД сообщает, количество подключений ограничено и оно было достигнуто.

Верхняя граница подключений контролируется ключем max_connections в конфигурации Postgres, и по умолчанию = 100. Почти каждый облачный Postgres поставщик, такой как Google Cloud Platform, Heroku ограничивают подключения очень осторожно, для больших баз данных выдавая лимит в 500 подключений, и для маленьких  гораздо меньше от 20 до 25.

С первого взгляда это может выглядеть контринтуитивно. Если проблема лимита подключений общеизвестна, почему бы просто не увеличить их кол-во, для избежания проблемы? 
Как и большинство вещей в компьютерах, решение не такое просто как может показаться с первого взгляда, и существует ряд факторов которые ограничивают максимальное число подключений которое можно иметь на практике. Некоторые очевидные, некоторые не очень. Давайте посмотрим поближе.

Наиболее прямое ограничение, но так же вероятно наименее важное - память. Postgres спроектирован вокруг процессной модели где центральный процесс Postmaster принимает входящие соединения и делает fork дочернего процесса и обработки этого запроса. Каждый из этих бекендов стартует с размером около 5Мб, но может расти в зависимости от данных которые он получает.
![[Pasted image 20230401214629.png]]

Поскольку в наши дни довольно легко приобрести систему с обилием памяти, потолок памяти не является ограничивающим фактором. Более тонкий и более важный аспект, что Postmaster и его бекенды используют разделяемую память для коммуникации, и части этой памяти являются глобальным бутылочным горлышком. Для примере, это структура которая отслеживает каждый входящий процесс и транзакцию:
```C
typedef struct PROC_HDR { 
/* Array of PGPROC structures (not including dummies for prepared txns) */ 
	PGPROC *allProcs;
/* Array of PGXACT structures (not including dummies for prepared txns) */ 
	PGXACT *allPgXact; ... 
}
extern PGDLLIMPORT PROC_HDR *ProcGlobal;
```

Операции которые могут выполняться в любом бекенде требуют весь список процессов или транзакций. Добавление нового процесса в массив процессов требуют исключительной блокировки 
```C
void ProcArrayAdd(PGPROC *proc) { 
	ProcArrayStruct *arrayP = procArray; 
	int index; 
LWLockAcquire(ProcArrayLock, LW_EXCLUSIVE); ... 
}
```

Так же GetSnapshotData часто вызывается множество раз для любой операции и должен проходить через каждый другой процесс в системе.
```C
Snapshot GetSnapshotData(Snapshot snapshot) { ProcArrayStruct *arrayP = procArray; ... 
/* * Spin over procArray checking xid, xmin, and subxids. The goal is * to gather all active xids, find the lowest xmin, and try to record * subxids. */ 
	numProcs = arrayP->numProcs; for (index = 0; index < numProcs; index++) { 
		... 
	} 
}
```

На обычном пути есть несколько таких узких мест которые Postgres использует для работы, и они конечно являются дополнением к обычному конфликту, который вы ожидаете найти вокруг системных ресурсов таких как ввод-вывод или CPU.
Кумулятивный эффект заключен в том, что в производительность пропорциональна кол-ву всех активных бекендов в системе.
Поэтому хотя это и может раздражать, что платформу такие как Google Cloud and Heroku ограничивают общее число подключений даже на очень больших серверах, они на самом деле пытаются помочь вам. Производительность в Postgres не надежна когда она масштабируется до большого кол-ва подключений. Как только вы начнете сталкиваться с большим лимитом подключений, например 500, правильным ответом будет вероятно не их увеличение, а попытка понять как эффективно эти подключения переиспользовать.

Техники эффективного использования подключений.