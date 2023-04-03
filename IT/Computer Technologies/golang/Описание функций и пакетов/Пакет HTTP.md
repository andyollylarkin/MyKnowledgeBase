---
aliases: [golang http package, servehttp, servemux]
tags: [go,golang,http,basic,basis]
---
<h6>01-04-2023</h6>
----------
## Handler интерфейс
В пакете http имеется следующий интерфейс
```go
type Handler interface{
	ServeHTTP(ResponseWriter, *Request)
}
```
Для самой простой реализации можно сделать примерно так:
```go
type home struct {}

func (h *home) ServeHTTP(w http.ResponseWriter, r *http.Request) {
	w.Write([]byte("Это моя домашняя страница"))
}
```
Но для этого для маршрута должна быть создана структура, что не всегда необходимо. 
Для избежания создания структуры можно использовать адаптер *HandlerFunc*
```go
// The HandlerFunc type is an adapter to allow the use of// ordinary functions as HTTP handlers. If f is a function  
// with the appropriate signature, HandlerFunc(f) is a  
//Handler that calls f.
type HandlerFunc func(ResponseWriter, *Request)  
  // ServeHTTP calls f(w, r).func (f HandlerFunc) 
func ServeHTTP(w ResponseWriter, r *Request) {  
   f(w, r)  
}
```
По сути можно создать функцию с любым именем, но соотв. сигнатуре **func(ResponseWriter, \*Request)**, после чего привести эту функцию к типу *HandlerFunc*
```go
func home(w http.ResponseWriter, r *http.Request) {
    w.Write([]byte("Моя домашняя страница!"))
}
// после чего приводим к типу
var h http.Handler // переменная типа интферфейса
h = http.HandlerFunc(home)
```
Как можно видеть из декларации *HandlerFunc* работает следующим образом: Тип обьявляется как функция с сигнатурой func(ResponseWriter, *Request). Сам тип имеет метод ServeHTTP с ресивером *HandlerFunc*. В теле метода ServeHTTP вызывается сам ресивер как **f(w,r)**. И после приведения нашей функции *home* к *HandlerFunc* наша функция получает метод *ServeHTTP* и сама становится ресивером. 

---
## Библиография
- [Интерфейс Handler](https://golangify.com/http-handler-interface)
- [HTTP маршрутизация](https://metanit.com/go/web/1.2.php)
