---
aliases: [Json Web Token, JW Token, JWT Token, JWT авторизация, go jwt, swaggo]
tags: [crypto, authentication, basis, cs, security]
---
<h6>25-09-2022</h6>
----------
## Определение
Метод авторизации на основе **Json Web Token**.

## Зачем?
![[Pasted image 20220925232158.png]]

0. Клиент обращается на сервер
1. Сервер генерирует его и отправляет в [[Cookie]]
> Для установки [[PHP сессии и Cookie|cookie]] сервер передает HTTP параметр Set-Cookie ([см. MDN](https://developer.mozilla.org/ru/docs/Web/HTTP/Cookies))

2. Далее клиент общается с сервером идентифицируя себя с помощью ID который выдал сервер

Далее в нашем сервисе появились другие сервисы (микросервисы, т.е. отдельные от нашего текущего приложения сервисы), или внешняя авторизация.

Допустим у нас есть сервер авторизации и сервер бизнес логики:
![[Pasted image 20220925232949.png]]
0. Клиент регистрируется на сервере
1. В ответ клиент некоторый id в системе
2. Клиент идет на сервер БЛ

Далее есть 2 проблемы:
1. **[[PHP сессии и Cookie|Cookie]]** устанавливается для определенного домена (поле <u>Domain</u> в <u>Set-Cookie</u>)
2. Серверу БЛ ничего не говорит наш ID, т.к. эта информация о нас которую мы можем получить по данному ID находится в "хранилище" сервера *Auth* и чтобы получить данные по этому ID нам нужно сходить из сервера *Domain Logic*. 

## Решение с помощью JWT
Решением может послужить JWT Token.
JWT токен имеет следующую структуру:

1. **HEADER**
2. **PAYLOAD**
3. **SIGNATURE**

*Сигнатура* это [[base64]] закодированные *заголовок* и *payload*, которые подписаны **секретным ключем** хранящимся на сервере.

Этот секретный ключ "расшарен" между сервером *Auth* и *Domain Logic*.
> По хорошему такой секрет нужно хранить и получать из хранилищ секретов, например HashiCorp Vault

Таким образом клиент:
1. Аутентифицируется на сервере *Auth* и получает в ответ <u>Access Token</u>
2. Идет с этим токеном на *Domain Logic*
3. Т.к. на сервере *Domain Logic* мы знаем ключ которым подписывался токен, мы можем его расшифровать и быть уверены, что клиент валиден.

> <span style="color: red">По сути разделение секретного ключа между серверами и есть тот принцип по которому мы можем ходить и расшифровывать сообщения между серверами.</span>

![[Pasted image 20230116210246.png]]

## Swag go заголовки OpenAPI авторизации для Golang

В api general description нужно указать определение:
> // @securityDefinitions.apiKey BearerAuth

Далее в эндпоинтах используется следующее определение

> // @Security BearerAuth

Данное определение говорит swag go о том, что для маршрута требуется авторизация которая определена в BearerAuth

---
## Библиография
- [Про JWT](https://gist.github.com/zmts/802dc9c3510d79fd40f9dc38a12bccfc)
- [JWT Introduction](https://jwt.io/introduction)
- [Зарегестрированные Claims в IANA](https://www.iana.org/assignments/jwt/jwt.xhtml)
- [RFC 7519 - JWT](https://www.rfc-editor.org/rfc/rfc7519.html#page-17)
- [Мой пример API с использованием swag go](https://github.com/andyollylarkin/go_api_swagger)
- [Максим Жашкевич - JWT авторизация API на Go](https://zhashkevych.medium.com/jwt-%D0%B0%D0%B2%D1%82%D0%BE%D1%80%D0%B8%D0%B7%D0%B0%D1%86%D0%B8%D1%8F-%D0%B4%D0%BB%D1%8F-%D0%B2%D0%B0%D1%88%D0%B5%D0%B3%D0%BE-api-%D0%BD%D0%B0-go-80325de8691b)
- [Swagger docs - Bearer](https://swagger.io/docs/specification/authentication/bearer-authentication/)