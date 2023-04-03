---
aliases: [go sync once, once]
tags: [go, golang,basis, sync]
---
<h6>17-02-2023</h6>
----------
Функция Once из пакета sync предназначена для единоразового исполнения некоторого кода. При повторном вызове колбек который передается в Once исполнен не будет.

> Once нет необходимости инициализировать, поскольку она следует идиоме полезного нулевого значения

> Once лучше обьявлять глобально(в рамках модуля), а не на уровне функции, поскольку при каждом новом вызове Once не будет запоминать свое предыдущее состояние. 

```go
var once sync.Once  
  
func main() {  
   sig := make(chan os.Signal, 1)  
   var wg sync.WaitGroup  
  
   signal.Notify(sig, syscall.SIGINT)  
  
   wg.Add(1)  
   go func() {  
      for {  
         <-sig  
         fmt.Println("BEFORE DOING ONE ACTION")  
         once.Do(func() {  
            printer()  
         })  
      }  
      wg.Done()  
   }()  
   wg.Wait()  
}  
  
func printer() {  
   fmt.Println("PRINT VALUE ONCE")  
}
```
Код выше единожды напечатает "PRINT VALUE ONCE". При повторном получении сигнала функция printer исполнена не будет.

---
## Библиография
- [Go doc sync.Once](https://pkg.go.dev/sync#Once)
