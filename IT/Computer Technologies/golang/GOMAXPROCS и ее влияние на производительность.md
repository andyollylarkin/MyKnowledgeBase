---
aliases: [go processors, goroutine performance]
tags: [go, golang, thread]
---
<h6>26-03-2023</h6>
----------
Существует следующий код
```go
func main() {  
   time := time2.Now()  
   var wg sync.WaitGroup  
   wg.Add(1)  
   go func() {  
      for i := 0; i < 1_000_000_000; i++ {  
         i = i  
      }  
      fmt.Println("go done")  
      wg.Done()  
   }()  
   for i := 0; i < 1_000_000_000; i++ {  
      i = i  
   }  
   fmt.Println("main done")  
   elapsed := time2.Since(time)  
   wg.Wait()  
   fmt.Println(elapsed.String())  
}
```

Из него мы можем явно увидеть как *GOMAXPROCS* влияет на производительность программы распараллеливая вычисления на нескольких ядрах процессора.
При *GOMAXPROCS=1* на моём компьютере выполнение занимает **526ms**. При *GOMAXPROCS=2* выполнение занимает **276ms**, что почти в 2 раза быстрее, посколько анонимная [[горутина]] работает параллельно основной [[Горутина|горутине]] main.

---
## Библиография
- 
