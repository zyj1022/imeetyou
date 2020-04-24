---
title: 设计模式之享元模式(Flyweight)
date: 2017-04-07 12:36:39
categories:
- frontend
tags:
- 设计模式
- js
---

## 介绍

享元(flyweight)模式是一种用于性能优化的模式，享元模式的核心是运用共享技术来有效支持大量细粒度的对象。

享元模式可以避免大量非常相似类的开销，在程序设计中，有时需要生产大量细粒度的类实例来表示数据，如果能发现这些实例除了几个参数以外，开销基本相同的话，就可以大幅度较少需要实例化的类的数量。如果能把那些参数移动到类实例的外面，在方法调用的时候将他们传递进来，就可以通过共享大幅度第减少单个实例的数目。

那么如果在JavaScript中应用享元模式呢？有两种方式：

- 第一种是应用在数据层上，主要是应用在内存里大量相似的对象上；
- 第二种是应用在DOM层上，享元可以用在中央事件管理器上用来避免给父容器里的每个子元素都附加事件句柄

<!-- more -->

## 享元与数据层

享元模式要求将对象的属性划分为内部状态与外部状态(状态在这里通常指属性)。享元模式的目标是尽量减少共享对象的数量，关于如何划分内部状态和外部状态，下面的几条经验提供了一些指引。

- 内部状态存储于对象内部。
- 内部状态可以被一些对象共享。
- 内部状态独立于具体的场景，通常不会改变。
- 外部状态取决于具体的场景，并根据场景而变化，外部状态不能被共享

说白点,就是先捏一个的原始模型，然后随着不同场合和环境,再产生各具特征的具体模型，很显然,在这里需要产生不同的新对象，所以Flyweight模式中常出现Factory模式，Flyweight的内部状态是用来共享的，Flyweight factory负责维护一个Flyweight pool(模式池)来存放内部状态的对象。

## 使用实例

**如下场景：**

图书馆有很多书，设计一个管理系统管理所有的借书信息

假定每个书的内容有：`title`、`author`、`id`、`site`

每本书被借出时间、借书人、归还日期、是否可用：

```
checkoutDate
checkoutMember
dueReturnDate
availability
```
我们可以将数据分成内部和外部两种数据，和book对象相关的数据（title, author 等）可以归结为内部属性，而（checkoutMember, dueReturnDate等）可以归结为外部属性。这样，如下代码就可以在同一本书里共享同一个对象了，因为不管谁借的书，只要书是同一本书，基本信息是一样的：

```
// 内部公共属性
var Book = function(title, author, id, ISBN) {
    this.title = title;
    this.author = author;
    this.id = id;
    this.ISBN = ISBN
}

```

### 定义基本工厂

让我们来定义一个基本工厂，用来检查之前是否创建该book的对象，如果有就返回，没有就重新创建并存储以便后面可以继续访问，这确保我们为每一种书只创建一个对象：

```
// book工厂
var BookFactory = (function(){
    var existingBooks = {};
    return {
        createBook: function(title, author, id, ISBN) {
           // 查找之前是否创建
           var existingBook = existingBooks[ISBN];
           if (existingBook) {
                   return existingBook;
               } else {
               // 如果没有，就创建一个，然后保存
               var book = new Book(title, author, id, ISBN);
               existingBooks[ISBN] =  book;
               return book;
           }
        }
    }
})();
```

### 管理外部状态

外部状态，相对就简单了，除了我们封装好的book，其它都需要在这里管理：

```
var BookRecord = (function() {
    var bookRecordDatabase = {};
    return {
        addBook: function(title, author, id, ISBN, checkoutDate, checkoutMember, dueReturnDate, availability) {
            var book = BookFactory.createBook(title, author, id, ISBN);
            bookRecordDatabase[id] = {
               checkoutMember: checkoutMember,
               checkoutDate: checkoutDate,
               dueReturnDate: dueReturnDate,
               availability: availability,
               book: book
           };
           console.log(bookRecordDatabase);
       },
       updateCheckoutStatus: function(bookID, newStatus, checkoutDate, checkoutMember, newReturnDate){
            var record = bookRecordDatabase[bookID];
            record.availability = newStatus;
            record.checkoutDate = checkoutDate;
            record.checkoutMember = checkoutMember;
            record.dueReturnDate = newReturnDate;
       },
       extendCheckoutPeriod: function(bookID, newReturnDate) {
           bookRecordDatabase[bookID].dueReturnDate = newReturnDate;
       },
       isPastDue: function(bookID) {
           var currentDate = new Date();
           return currentDate.getTime() > Date.parse(bookRecordDatabase[bookID].dueReturnDate);
       }
   }
})();
```

通过这种方式，我们做到了将同一种图书的相同信息保存在一个bookmanager对象里，而且只保存一份；相比之前的代码，就可以发现节约了很多内存。

## 总结

享元模式是为解决性能问题而生的模式，这跟大部分模式的诞生原因都不一样。在一个存在
大量相似对象的系统中，享元模式可以很好地解决大量对象带来的性能问题。

-----

> 参考引用资料
>
> 《JavaScript设计模式与开发实践》
>
> [汤姆大叔的博客——深入理解JavaScript系列](http://www.cnblogs.com/TomXu/archive/2011/12/15/2288411.html)
