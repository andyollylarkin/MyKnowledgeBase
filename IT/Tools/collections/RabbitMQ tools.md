1. amqp-tools - пакет который содержит утилиты для консьюминга и продьюсинга сообщений в RabbitMQ
```shell
# Пример скрипта
#!/bin/bash 
tools=$(apt list --installed|grep -Eo 'amqp-tools' 2>/dev/null)
 MESSAGE=$1
 if [[ -z $tools ]]; then
     apt install -y amqp-tools;
fi
amqp-publish --url='amqp://guest:guest@127.0.0.1:5672' -r 'jobs.hostjobs.worker' -e 'jobs' -b ${MESSAGE};

```