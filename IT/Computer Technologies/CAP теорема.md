---
aliases: [CAP теорема, CAP, highload, распределенные системы]
tags: [microservices, CAP_theoreme, consistency, ]
---
# CAP теорема
<h6>14-08-2022</h6>
----------
## Определение

### Теорема CAP

[![](https://github.com/donnemartin/system-design-primer/raw/master/images/bgLMI2u.png)](https://github.com/donnemartin/system-design-primer/blob/master/images/bgLMI2u.png)  
_[Источник: пересмотренная теорема CAP.](http://robertgreiner.com/2014/08/cap-theorem-revisited)_

В распределенной компьютерной системе вы можете поддерживать только две из следующих гарантий:

-   **Непротиворечивость** . Каждое чтение получает самую последнюю запись или ошибку.
-   **Доступность** — на каждый запрос приходит ответ, без гарантии того, что он содержит самую последнюю версию информации.
-   **Допуск к разбиению** — система продолжает работать, несмотря на произвольное разбиение на разделы из-за сетевых сбоев.

_Сети ненадежны, поэтому вам необходимо поддерживать устойчивость к разделам. Вам нужно найти компромисс между согласованностью и доступностью программного обеспечения._

#### [](https://github.com/donnemartin/system-design-primer#cp---consistency-and-partition-tolerance)CP - непротиворечивость и устойчивость к разделам

Ожидание ответа от разделенного узла может привести к ошибке тайм-аута. CP — хороший выбор, если вашему бизнесу требуются атомарные операции чтения и записи.

#### [](https://github.com/donnemartin/system-design-primer#ap---availability-and-partition-tolerance)AP — доступность и устойчивость к разделам

Ответы возвращают наиболее доступную версию данных, доступных на любом узле, которая может быть не самой последней. Записи могут занять некоторое время для распространения после разрешения раздела.

AP — хороший выбор, если бизнесу необходимо обеспечить [возможную согласованность](https://github.com/donnemartin/system-design-primer#eventual-consistency) или когда система должна продолжать работать, несмотря на внешние ошибки.
---
## Библиография
-   [Пересмотренная теорема CAP](http://robertgreiner.com/2014/08/cap-theorem-revisited/)
-   [Простое английское введение в теорему CAP](http://ksat.me/a-plain-english-introduction-to-cap-theorem)
-   [CAP Часто задаваемые вопросы](https://github.com/henryr/cap-faq)
-   [Теорема CAP](https://www.youtube.com/watch?v=k-Yaq8AHlFA)
