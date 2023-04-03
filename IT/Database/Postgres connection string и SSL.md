---
aliases: [postgres dsn, dsn, postres connection, libpq connect]
tags: [pg, database, postgres, postgresql, db, basic, basis]
---
<h6>01-04-2023</h6>
----------

## Описание настройки SSL для СУБД
Сервер [[СУБД]] PostgreSQL поддерживает подключение через [[SSL-TLS|ssl]]. Для настройки необходимо сгенерировать сертификат и ключ сервера. Далее необходимо в настройках указать в postgresql.conf следующие параметры:
1. ssl=on
2. ssl_cert_file='path_to_cert'
3. ssl_key_file='path_to_key'

> Так же поддерживается mutual [[SSL-TLS|TLS]]

> После настройки [[SSL-TLS|SSL]] на сервере и клиенту удалось выполнить подключение с помощью [[SSL-TLS|SSL]], через TCPDUMP можно очевидно увидеть, что данные более не представлены в открытом виде, и всё зашифровано.

## PostgreSQL DSN
Postgres поддерживает подключение через *connection string*. Это специфический вид [[URI]] имеющий следующий вид:

> postgresql://user:password@host:port/db_name?options

Или в формате key-value

> host=host_ip port=port_num dbname=db_name other_param=param_value




---
## Библиография
- [PostgresPRO - libpq connect](https://postgrespro.ru/docs/postgresql/14/libpq-connect?lang=en#LIBPQ-CONNSTRING)
- [PostgresPRO - настройка SSL](https://postgrespro.ru/docs/postgresql/13/ssl-tcp#SSL-SETUP)
- [PSQL connection string and DSN](https://tapoueh.org/blog/2019/09/postgres-connection-strings-and-psql/)