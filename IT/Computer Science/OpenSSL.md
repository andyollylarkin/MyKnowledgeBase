---
aliases: [x509, x.509, ssl, tls,]
tags: [network, cipher, http, secutiry, encryption]
---
# OpenSSL
<h6>19-03-2022</h6>
----------
## Определение
Утилита для работы с TLS

## Описание
Утилита для работы с TLS

## Перечень команд
-   **genpkey** (заменяет **genrsa**, **gendh** и **gendsa**) — генерирует приватные ключи
-   **req** — утилита для создания запросов на подпись сертификата и для создания самоподписанных сертификатов PKCS#10
-   **x509** — утилита для подписи сертификатов и для показа свойств сертификатов
-   **rsa** — утилита для работы с ключами RSA, например, для конвертации ключей в различные форматы
-   **enc** — различные действий с симметричными шифрами
-   **pkcs12** — создаёт и парсит файлы PKCS#12
-   **crl2pkcs7** — программа для конвертирования CRL в PKCS#7
-   **pkcs7** — выполняет операции с файлами PKCS#7 в DER или PEM формате
-   **verify** — программа для проверки цепей сертификатов
-   **s_client** — команда реализует клиент SSL/TLS, который подключается к удалённому хосту с использованием SSL/TLS. Это очень полезный инструмент диагностики для серверов SSL
-   **ca** — является минимальным CA-приложением. Она может использоваться для подписи запросов на сертификаты в различных формах и генерировать списки отзыва сертификатов. Она также поддерживает текстовую базу данных выданных сертификатов и их статус
-   **rand** — эта команда генерирует указанное число случайных байтов, используя криптографически безопасный генератор псевдослучайных чисел (CSPRNG)
-   **rsautl** — команда может быть использована для подписи, проверки, шифрования и дешифрования данных с использованием алгоритма RSA
-   **smime** — команда обрабатывает S/MIME почту. Она может шифровать, расшифровывать, подписывать и проверять сообщения S/MIME

## Процесс создания SSL(TLS) сертификатов
1. Создание корневого приватного ключа (итогом является корневой приватный ключ, который используется для подписи остальных сертификатов) 
2. --> создание корневого сертификата (итогом является корневой сертификат который устанавливается на всех компьютерах на которых необходимо доверие к подписанным нами сертификатам) 
3. --> создание приватного ключа конкретного домена (итого является приватный ключ для домена, который мы будем использовать на оконечном сервере) 
4. --> создание запроса на подпись (итогом является файл запроса на подпись который можно отправить в центр сертификации для подписи )


---
## Библиография
- [Подробный гайд по TSL & openssl](https://hackware.ru/?p=12982)