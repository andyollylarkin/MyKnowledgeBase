---
aliases: [Связаность, связность, зацепление, cohesion, coupling]
tags: [concept, ]
---
# Cohesion and coupling
<h6>01-12-2022</h6>
----------
## Определение
**Cohesion (Связность)** -  мера того, насколько некоторая группа классов или модулей (*на более высоком уровне абстракции*) следует одной цели для которых они были созданы. Иными словами - мера силы взаимосвязи элементов внутри модуля.
**Coupling (Зацепление)** - мера того насколько разные модули зависят от других модулей в своей работе.

> Далее под модулями понимаются как отдельно взятые классы на уровне кода, так и модули как группы классов.

![[Pasted image 20221201012041.png]]

> Многие [[Паттерн|паттерны]] многослойной архитектуры в частности [[MVC]], [[MVP]], [[MVVM]] нацелены как раз на уменьшение зацепления между модулями

## Почему высокое зацепление это плохо
Сильное (высокое) зацепление рассматривается как серьезный недостаток, поскольку затрудняет понимание логики модулей (нужно понять множество отдельно взятых частей, чтобы понять как модуль работает в целом), их модификацию, автономность тестирования (чем больше у модуля прямых зависимостей, тем сложнее для него написать тест), а так же переиспользование по отдельности. 

## Методы уменьшения зацепления (decoupling)
- [[Шаблон проектирования|шаблоны проектирования]]
- [[Инкапсуляция|Сокрытие информации]]
- [[Инверсия управления]] и в частности [[Внедрение зависимостей]]



## От Сергея Протько (Fes0r)
>Ну, самое главное что надо понять это что же все таки такое coupling. Ну мол вот у тебя есть две штуки, как понять что между ними есть вообще каплинг, как его "померять". Если дать определение что "каплинг это мера как один объект влияет на другой" то как бы сложно, а вот если влияние это определять изменениями чего-то - то уже чуть проще. Виды каплинга по факту просто "что может поменяться, как влияет". Степень связанности и насколько каплинг проблема - какова вероятность что чет поменяется и как это отслеживать.С этой точки зрения "message coupling" - поменяли сигнатуру - легко отслеживать. data coupling - в зависимости от ситуации может быть легко. global coupling - например одна штука пишет в глобальную переменную другая читает - тут прям сложно, неявные [[Интерфейс|интерфейсы]] оч сложно отслеживать. Какой-нибудь temporal coupling - мол "ввлияет че в каком порядке происходит" - с этим можно бороться и уменьшать риски. Каплинг между системой и организацией - об этом вообще часто не думают. ключевое что связанность есть обязательная/оправданная и есть "случайная", которая просто образавалась но не имеет под собой реальной необходимости. И вот такой связанности обычно больше всего. Это может быть просто степень зависимости двух компонентов между собой (например передавать весь объект вместо айдишки в метод, что потребует везде доставать этот объект и т.д.). А так в целом википедии достаточно (можно книжку про структурный дизайн почитать но будет тяжко) + можно каких умных ребят послушать (Corey Haines, Curtis Cooley, Dale Emery, J. B. Rainsberger, Jim Weirich, Kent Beck, Nat Pryce, Ron Jeffries.)

---
## Библиография
- 
