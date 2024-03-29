---
aliases: [csrf, cross site resource forgery]
tags: [net, network, http, security, basis, foundation]
---
<h6>08-02-2023</h6>
----------
## Определение
**CSRF** - межсайтовая подделка запроса. Форма атаки на сайт или веб приложение. С помощью подделки запроса злоумышленник заставляет жертву обманом выполнить нежелательный запрос.

## Как работает
Атака будет работать только в том случае, если жертва авторизована на ресурсе куда направляется поддельный запрос.
Выполнение CSRF-атаки состоит из двух основных частей.

1.  Злоумышленник заставляет жертву щёлкнуть на ссылку или загрузить веб-страницу. Он намеренно заманивает пользователя перейти по ссылке, используя методы социальной инженерии.
2.  Отправляет «поддельный» или выдуманный запрос в браузер жертвы. Вредоносная ссылка отправит запрос в веб-приложение, однако будет включать в себя значения, которые выгодны злоумышленнику.
![[Pasted image 20230208000504.png]]
## Понимание атаки
Следует понимать, что защищается не запрос внутри одного веб приложения, а запросы на ресурс со стороны сторонних ресурсов.
Допустим у нас есть веб приложение банка. Злоумышленник оставляет <u>на стороннем форуме</u> следующую ссылку: http://bank.example.com/?account=Alice&amount=1000000&for=Bob
Когда этот запрос приходит в наш банк, он придет без **CSRF** токена и будет отклонен. С другой стороны с веб страниц нашего банка будет приходить запрос с наличием такого токена, поскольку мы его туда и встроили и соответственно запрос будет легитимным.

---
## Библиография
- [Статья про CSRF](https://www.nic.ru/help/token-csrf-kak-zashita-ot-csrf-atak_11098.html#:~:text=%D0%9A%D0%B0%D0%BA%20%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%D0%B5%D1%82%20CSRF%2D%D0%B0%D1%82%D0%B0%D0%BA%D0%B0&text=%D0%92%D1%8B%D0%BF%D0%BE%D0%BB%D0%BD%D0%B5%D0%BD%D0%B8%D0%B5%20CSRF%2D%D0%B0%D1%82%D0%B0%D0%BA%D0%B8%20%D1%81%D0%BE%D1%81%D1%82%D0%BE%D0%B8%D1%82%20%D0%B8%D0%B7,%D0%B2%D1%8B%D0%B4%D1%83%D0%BC%D0%B0%D0%BD%D0%BD%D1%8B%D0%B9%20%D0%B7%D0%B0%D0%BF%D1%80%D0%BE%D1%81%20%D0%B2%20%D0%B1%D1%80%D0%B0%D1%83%D0%B7%D0%B5%D1%80%20%D0%B6%D0%B5%D1%80%D1%82%D0%B2%D1%8B.)