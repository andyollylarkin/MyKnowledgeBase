---
aliases: [go memory, go heap, go internal, go stack,]
tags: [go, golang, basis,cs,concept,foundation]
---
<h6>05-02-2023</h6>
----------
## Две области памяти куча и стек

> Стек самоочищается, при выходе из функции. Куча в свою очередь должна быть очищена Garbage Collector'ом.


![[Pasted image 20230205223311.png]]

### Стек
Стек в большинстве языков программирования чаще всего называется call stack. Это LIFO структура данных которая хранит аргументы локальные переменные и другие данные исполняемой фукнции. Каждая функция при вызове помещает новый стековый фрейм в стек и каждый возврат из функции извлекает данный фрейм.
![[Pasted image 20230205223017.png]]
> Поскольку потоки управляются ОС, обьем доступной памяти для стека обычно фиксирован. В Linux по умолчанию под стек одного потока отводится 8Мб
> Именно по причине исчерпания этой памяти программа которая вошла в бесконечную рекурсию - падает.

### Куча (heap)
Куча - более сложная область памяти.

> Куча в памяти не имеет ничего общего с одноименной структурой данных

>На уровне Linux память в куче выделяется функций libc - **malloc** и освобождается **free**

> Все [[Горутина|горутины]] имеют общую кучу
![[Pasted image 20230205223549.png]]

![[Pasted image 20230205223649.png]]
## Бенчмарки
Бенчмарк выделения памяти можно запустить следующим образом
```shell
go test -bench=. -benchmem
```
При этом в выводе бенчмарка мы получим следующее описание: allocs/op
Это означает сколько операций выделения памяти в куче было произведено.

---
## Библиография
- [Механика escape-analysis](https://habr.com/ru/post/497994/)
- [Go memory allocations understnding](https://medium.com/eureka-engineering/understanding-allocations-in-go-stack-heap-memory-9a2631b5035d)