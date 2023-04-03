---
aliases: [repository, collection like repository, storage like repository, репозиторий, паттерн репозиторий]
tags: [oop, concept, patterns, architecture, ddd, orm, design]
---

<h6>21-11-2022</h6>
----------
## Определение
Репозиторий - это слой абстракции, инкапсулирующий в себе **всё, что относится к способу хранения** данных. Назначение: **Разделение бизнес-логики от деталей реализации слоя доступа к данным**. 

## Описание
Паттерн Репозиторий стал популярным благодаря [[DDD]] (Domain Driven Design).
В противоположность к Database Driven Design в [[DDD]] разработка начинается с проектирования бизнес логики, принимая во внимание только особенности предметной области и игнорируя все, что связано с особенностями базы данных или других способов хранения данных. Способ хранения бизнес объектов реализуется во вторую очередь. 

Применение данного паттерна не предполагает создание только одного объекта репозитория во всем приложении. Хорошей практикой считается создание отдельных репозиториев для каждого бизнес-объекта или контекста, например: OrdersRepository, UsersRepository, AdminRepository.

## Пример

```c#
public interface IPostsRepository
{
    void Save(Post mypost);
    Post Get(int id);
    PaginatedResult<Post> List(int skip,int pageSize);
    PaginatedResult<Post> SearchByTitle(string title,int skip,int pageSize);
}
```

- Метод `save` определяет какой объект передан - новый или существующий, в зависимости от этого использует команду `insert` или `update`.
- Методы `List` и `SearchByTitle` возвращают список постов, в котором указывается информация об отобранной странице.


## Generic Repository это антипаттерн

Не существует определенных правил каким должен быть интерфейс репозитория - это всецело зависит от предметной области. Однако есть класс простых приложений, которые работают с данными одинаково. И когда нужно получить простой универсальный способ работы с данными - это единственный случай, когда использование обобщенного репозитория оправдано. 

```c#
public interface IRepository<T>
{
   IEnumerable<T> GetAll();
   T GetByID(int id);   
   void Add(T entity);
   void Update(T entity);
   void Delete(T entity);
}
```

## Репозиторий и ORM

Возможно у вас возник вопрос: Зачем использовать репозиторий, если я использую ORM? 

Действительно, ORM позволяет:

- работать с данными, оперируя бизнес-объектами (POCO).
- легко сменить поставщика данных (SQL Server на MySql и т.д.)

Однако может быть масса случаев, когда хранение данных представляет собой нечто более сложное или специфичное, чем просто ORM. И тогда такой слой данных инкапсулируется с помощью паттерна репозиторий:

- Если вы хотите сменить поставщика данных на NoSql базу данных или файлы.
- Вы можете использовать несколько поставщиков данных. Например, MySql для данных и файловую систему для хранения изображений. 
- Строго говоря [модель ORM не является моделью предметной области](http://www.sapiensworks.com/blog/post/2012/04/07/Just-Stop-It!-The-Domain-Model-Is-Not-The-Persistence-Model.aspx). И в случае когда это действительно разные вещи, а используется только модель ORM, то это уже не [[DDD]].

Если ваш репозиторий возвращает `IQueryable`, то это ошибка, т.к. репозиторий не должен возвращать что-либо относящееся к способу хранения. Правильно было бы `IEnumerable`.

## Преимущества паттерна Репозиторий

- Выделение общей логики (DRY): проверки, значения по умолчанию, логирование и т.д..
- Независимость бизнес-логики от способа хранения. Используя репозиторий, вы обращаетесь с коллекциями бизнес объектов (POCO), но не с database related объектами, не с Data Access Objects. Возможность использовать разные способы хранения:  ORM, rdbms, cloud storage, file system etc, заменять их и комбинировать. 
- Работая через интерфейсы вы можете создать несколько реализаций репозитория. Это помогает при тестировании (можно передавать заглушку репозитория при тестировании бизнес-логики) и при изменении способа хранения данных. Конкретная реализация репозитория может регистрироваться в [[Внедрение зависимостей|IoC]] и т.о. ни один модуль приложения за исключением точки входа не будет связан с модулем реализации репозитория. Приложение будет работаеть с интерфейсом репозитория и объектами бизнес-логики.

## Репозиторий и DAL (Data Access Layer, Persistence Layer)

Репозиторий - это высокоуровневая абстракция доступа к данным. Интерфейс каждого конкретного репозитория (контракт) определяется в слое бизнес-логики наряду с классами предметной области. Реализация каждого репозитория находится в слое доступа к данным DAL. Соответственно DAL состоит из реализации каждого репозитория, ORM-специфичных классов, сущностей, классов-сопоставлений (mapping), контекстов данных и т.д.

## Ссылки

Источник: Серия статей by Mike Mogosanu из его блога https://blog.sapiensworks.com:

- [The Repository Pattern Explained (For Dummies)](https://blog.sapiensworks.com/post/2014/06/02/The-Repository-Pattern-For-Dummies.aspx) published on 02 June 2014 
- [The Generic Repository Is An Anti-Pattern](https://blog.sapiensworks.com/post/2012/03/05/The-Generic-Repository-Is-An-Anti-Pattern.aspx) published on 05 March 2012
- [The Repository Pattern Vs ORM](https://blog.sapiensworks.com/post/2012/04/15/The-Repository-Pattern-Vs-ORM.aspx) published on 15 April 2012
- [Do We Need The Repository Pattern?](https://blog.sapiensworks.com/post/2012/10/10/Do-We-Need-The-Repository-Pattern.aspx) published on 10 October 2012
- [Repository vs DAO](https://blog.sapiensworks.com/post/2012/11/01/Repository-vs-DAO.aspx) published on 01 November 2012
- [Repository vs Everything Else](https://blog.sapiensworks.com/post/2013/01/18/Repository-vs-Document-Store-vs-Everything-Else.aspx) published on 18 January 2013
- [Repository Pattern and IQueryable](https://blog.sapiensworks.com/post/2013/01/24/Repository-Pattern-and-IQueryable.aspx) published on 24 January 2013
- [Say "Yes" To The Repository In Your DAL](https://blog.sapiensworks.com/post/2013/06/20/Say-Yes-To-The-Repository-In-Your-DAL.aspx) published on 20 June 2013
- [Tips And Tricks For The Repository Pattern](https://blog.sapiensworks.com/post/2013/10/07/Tips-And-Tricks-For-The-Repository-Pattern.aspx) published on 07 October 2013


На момент написания этой заметки статьи Mike размещались в категории Repository, затем он переделал свой блог и эти статьи попали в категорию Best Practces, что охватывает и другие вопросы, выходящие за рамки темы Паттерн Репозиторий. Поэтому привожу здесь [ссылку на версию статей 2014 года](http://web.archive.org/web/20140905114045/http://www.sapiensworks.com/blog/category/Repository.aspx).

---
## Библиография
- [Gist - описание паттерна](https://gist.github.com/andyollylarkin/395e3a5712a78fc9644859682c9f4ca7)
