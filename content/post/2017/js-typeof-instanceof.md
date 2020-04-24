---
title: typeof与instanceof的区别
date: 2017-03-26T17:24:36+08:00
tags: ["frontend", "typeof", "instanceof"]
categories: ["frontend"]
---

typeof和instanceof都可以用来判断变量，它们的用法有很大区别：

* **typeof：返回一个变量的基本类型，检测的是基本数据类型**
* **instanceof：返回的是一个布尔值，检测的是引用类型**


## typeof

typeof会返回一个变量的基本类型，只有以下几种：number,boolean,string,object,undefined,function；例：

<!-- more -->

```
console.log(typeof 1); // number
console.log(typeof "abc"); // string
console.log(typeof true); // boolean
console.log(typeof a); // undefined
console.log(typeof new Object()); // object
console.log(typeof null); // object
console.log(typeof []); // object
```
可以看到 `typeof` 无法判断数组、null，不管是数组还是对象，都会返回 object 

更多方法具体可查看 [《Javascript基础之数组》](http://zyj1022.github.io/posts/frontend/2017/js-array-base.html)—数组检测方法



## instanceof

instanceof返回的是一个布尔值，如：

```
var a = {};
console.log(a instanceof Object);  // true
var b = [];
console.log(b instanceof Array);  // true
```

instanceof操作符是检测对象的原型链是否指向构造函数的prototype对象,所以可以用来判断数组。

需要注意的是，**instanceof只能用来判断对象和函数，不能用来判断字符串和数字等**，如：

```
var b = '123';
console.log(b instanceof String);  // false
console.log(typeof b);  // string

var c = new String("123");
console.log(c instanceof String);  // true
console.log(typeof c);  // object

更多格式转换技巧查看：[格式转换 – Kindle伴侣](https://kindlefere.com/skills/convert)



