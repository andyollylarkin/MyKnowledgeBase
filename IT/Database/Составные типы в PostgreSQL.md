---
aliases: [составной тип]
tags: [db, database, sql, type, plpgsql]
---
<h6>01-12-2022</h6>
----------
## Определение
Составной тип является типом-контейнером в системе типов **PostgreSQL**. Составной тип это такой тип, который может содержать в себе несколько значений других типов. По сути составной тип это просто список имен полей и их типов данных.

### Создание составного типа
```sql
CREATE TYPE coord AS  
(  
    x int,  
    y int  
);
```
После определения такого типа, его можно использовать как тип в колонке таблицы.

### Обращение к полям составного типа
```sql
SELECT (sq).r.x FROM (  
SELECT ROW(10, 12)::coord as r) as sq;
```
> (sq) выше заключен в скобки поскольку синтаксис обращения к колонке таблицы и полю типа неотличимы, поэтому выбор через точку из таблицы мы заключили в скобки.

### Разбиение составного типа по колонкам в запросе
```sql
SELECT (sq).r FROM (  
SELECT ROW(10, 12)::coord as r) as sq;
```
Даст результат:
![[Pasted image 20221201032405.png]]
```sql
SELECT (sq).r.* FROM (  
SELECT ROW(10, 12)::coord as r) as sq;
```
Даст результат:
![[Pasted image 20221201032502.png]]
> [Подробности](https://postgrespro.ru/docs/postgrespro/9.5/rowtypes#rowtypes-usage)


### Конструирование составного типа с помощью ключевого слова ROW
```sql
SELECT ROW(10, 12)::coord
```

### Тип RECORD в plpgsql

*имя RECORD;*
Переменная не имеющая определенной структуры. Приобретает фактическую структуру от строки, которая присваивается командами *SELECT* или *FOR*.
```sql
DECLARE r RECORD;
-- ...
FOR r IN SELECT * FROM some_table() LOOP
	r.some_table_field -- обращение к элементу(столбцу) строки присвоеной из выборки some_table
END LOOP;
```

> Если нужно присвоить типу *RECORD* > 1 столбца (чего нельзя сделать в подзапросе, потому что подзапрос может вернуть только один столбец), то нужно использовать **SELECT INTO**
```sql
DECLARE r RECORD;
SELECT * INTO RECORD FROM some_table;
```

>см. [Тип RECORD](https://postgrespro.ru/docs/postgresql/13/plpgsql-declarations#PLPGSQL-DECLARATION-RECORDS)

---
## Библиография
- [PostgresPro doc - Система типов](https://postgrespro.ru/docs/postgresql/14/extend-type-system)
- [PostgresPro doc - Составные типы](https://postgrespro.ru/docs/postgrespro/9.5/rowtypes)
- [PosgresPro doc - конструирование табличных строк](https://postgrespro.ru/docs/postgrespro/9.5/sql-expressions#sql-syntax-row-constructors)
