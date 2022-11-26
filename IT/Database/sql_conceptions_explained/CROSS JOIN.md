```sql
SELECT * FROM (
	SELECT generate_series('2022-01-01'::date, '2022-01-02'::date, '1 day'::INTERVAL)::date) sq1
JOIN  
(SELECT 
 generate_series('2022-01-03'::date, '2022-01-04'::date, '1 day'::INTERVAL)::date) sq2 ON true

--- эквивалентно
SELECT * FROM (SELECT generate_series('2022-01-01'::date, '2022-01-02'::date, '1 day'::INTERVAL)::date) sq1
CROSS JOIN  
(SELECT generate_series('2022-01-03'::date, '2022-01-04'::date, '1 day'::INTERVAL)::date) sq2 
```

Результат:
![[Pasted image 20221018230003.png]]