---
aliases: [midnight commander, mc]
tags: [mc, tools, config, configuration, folder, explorer]
---
# Настройка расширений в MC
<h6>19-03-2022</h6>
----------
## Как он открывает файлы:

regex/i/\.(do[ct]|wri|docx)$

 Open=/usr/local/Cellar/midnight-commander/4.8.25/libexec/mc/ext.d/doc.sh open msdoc

 View=%view{ascii} /usr/local/Cellar/midnight-commander/4.8.25/libexec/mc/ext.d/doc.sh view msdoc

type/^Microsoft\ Word

 Open=/usr/local/Cellar/midnight-commander/4.8.25/libexec/mc/ext.d/doc.sh open msdoc

 View=%view{ascii} /usr/local/Cellar/midnight-commander/4.8.25/libexec/mc/ext.d/doc.sh view msdoc

В Open= запускается скрипт doc.sh с параметром open или view (это параметры для case далее в скрипте doc.sh)

**Сам скрипт запускает проверку case** 
```
208 case "${action}" in

209 view)

210     do_view_action "${filetype}"

211     ;;

212 open)

213     ("\$\{MC_XDG_OPEN\}" "\$\{MC_EXT_FILENAME\}" >/dev/null 2>&1) || \

214         do_open_action "${filetype}"

215     ;;

216 *)

217     ;;

218 esac
```

**view на просмотр open на открытие. Если открытие то запускает функцию do_open_action**
```
 **do_view_action() {**

 38 **filetype**=$1

 39

 40     case "${filetype}" in

 41     ps)

 42         ps2ascii "${MC_EXT_FILENAME}"

 43         ;;

 44     pdf)

 45         pdftotext -layout -nopgbrk "${MC_EXT_FILENAME}" -

 46         ;;

case проверка проверяет значение переданной переменной из Open. Т. е. $filetype

```




>Сам $filetype может быть pdf, msdoc и т.д. и при совпадении выполняется код запуска программы которая может открыть файл. Здесь и нужно редактировать код, запуская необходимую программу и передавая ей файл