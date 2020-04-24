---
title: Javascript定义类（class）的三种方法
date: 2017-03-23 09:22:23
categories:
- frontend
tags:
- js
- class
- frontend
---


在面向对象编程中，类（class）是对象（object）的模板，定义了同一组对象（又称"实例"）共有的属性和方法。

Javascript语言不支持"类"，但是可以用一些变通的方法，模拟出"类"。


## 1、构造函数法

第一种是经典方法，它用构造函数模拟"类"，在其内部用this关键字指代实例对象。

```
function Dog() {
　　this.name = "旺财";
}
```

生成实例的时候，使用new关键字。

```
var dog = new Dog();
console.log(dog.name); // 旺财
```

类的属性和方法，还可以定义在构造函数的prototype对象之上。

<!-- more -->

```
Dog.prototype.wow = function(){
　 console.log("汪～汪～汪！");
}
```
关于这种方法的详细介绍，请看《Javascript 面向对象编程》，这里就不多说了。

它的主要缺点是，比较复杂，用到了this和prototype，编写和阅读都很费力。

## 2、Object.create()法

在ECM5中，加入一个新的方法 `Object.create()`,用这个方法，"类"就是一个对象，不是函数。

```
var Dog = {
	this.name = "旺财";
    wow: function() {
		console.log("汪～汪～汪！");
	}
}
```

然后，直接用Object.create()生成实例，不需要用到new。

```
var dog = Object.create(Dog);
console.log(dog.name); // 旺财
dog.wow(); // 汪～汪～汪！
```

这种方法不能实现私有属性和私有方法，实例对象之间也不能共享数据。

## 3、BlackScript 法

荷兰程序员Gabor de Mooij提出了一种比Object.create()更好的[新方法](http://www.gabordemooij.com/index.php?p=/blackscript)。

事实上就是用一个对象模拟类，在这个类里，定义一个构造函数 createNew() 用来生成实例。

```
var Dog = {
   createNew: function(){
      //some code
	}
}
```
然后，在 createNew() 里面，定义一个实例对象，把这个实例对象作为返回值

```
var Dog = {
   createNew: function(){
      var dog = {};
	  dog.name = "旺财";
      dog.wow = function() {
         console.log("汪～汪～汪！");
      }
      return dog;
	}
}
```
使用方法

```
var dog1 = Dog.createNew();
dog1.wow(); // 汪～汪～汪！
```

这种方法简单易学，可以实现OOP的特性，继承、私有属性和方法、数据共享。


### 继承

比如让 Dog 继承 Animal,只要在 Dog的 createNew()方法中，调用后者的createNew()方法即可。

```
var Animal = {
　　　　createNew: function(){
　　　　　　var animal = {};
　　　　　　animal.sleep = function(){ 
				console.log("睡懒觉");
		  };
　　　　　　return animal;
　　　　}
};
```

然后，在 Dog 的 createNew() 方法中，调用 Animal 的 createNew() 方法。


```
var Dog = {
   createNew: function(){
      var dog = Animal.createNew();
	  dog.name = "旺财";
      dog.wow = function() {
         console.log("汪～汪～汪！");
      }
      return dog;
	}
}
```

这样得到的 Dog 实例，就会同时继承 Dog 类和Animal类。

```
var dog1 = Dog.createNew();
dog1.sleep() // 睡懒觉 
dog1.wow() // 汪～汪～汪！

```

### 私有属性和方法

在 createNew() 方法中，只要不是定义在 dog 对象上的方法和属性，都是私有的。

```
var Dog = {
   createNew: function(){
      var dog = {};
      var sound = "汪汪汪";
	  dog.name = "旺财";
      dog.wow = function() {
         console.log(sound);
      }
      return dog;
	}
}
```

上例的内部变量 sound，外部无法读取，只有通过 dog 的公有方法 wow() 来读取。

```
var dog1 = Dog.createNew();
dog1.name // 旺财
dog1.sound // undefined 
dog1.wow() // 汪汪汪
```
### 数据共享

有时候，我们需要所有实例对象，能够读写同一项内部数据。

这个时候，只要把这个内部数据，封装在类对象的里面、createNew()方法的外面即可。


```
var Dog = {
   sound: "汪汪汪",
   createNew: function(){
      var dog = {};
	  dog.name = "旺财";
      dog.sound = function() {
         console.log(Dog.sound);
      }
      dog.changSound = function(para){
         Dog.sound = para
      }
      return dog;
	}
};
```
生成两个实例

```
var dog1 = Dog.createNew();
var dog2 = Dog.createNew();
dog1.sound(); // 汪汪汪
dog2.sound(); // 汪汪汪
```
这时，如果有一个实例对象，修改了共享的数据，另一个实例对象也会受到影响。

```
dog2.changSound("呱呱呱");
dog1.sound(); // 呱呱呱

```

----

> 本文转载引用自 [Javascript定义类（class）的三种方法](http://www.ruanyifeng.com/blog/2012/07/three_ways_to_define_a_javascript_class.html)



