---
title: 设计模式之装饰者模式(Decorator)
date: 2017-04-21 9:16:39
categories:
- frontend
tags:
- 设计模式
- js
---

## 介绍

装饰者提供比继承更有弹性的替代方案。 装饰者用用于包装同接口的对象，不仅允许你向方法添加行为，而且还可以将方法设置成原始对象调用（例如装饰者的构造函数）。

装饰者用于通过重载方法的形式添加新功能，该模式可以在被装饰者前面或者后面加上自己的行为以达到特定的目的。

## 定义

给对象动态地增加职责的方式称为装饰者(decorator)模式。

装饰者模式能够在不改变对象自身的基础上，在程序运行期间给对象动态地添加职责。跟继承相比，装饰者是一种更轻便灵活的做法，这是一种“即用即付”的方式。

<!--more-->

## 使用实例

那么装饰者模式有什么好处呢？前面说了，装饰者是一种实现继承的替代方案。当脚本运行时，在子类中增加行为会影响原有类所有的实例，而装饰者却不然。取而代之的是它能给不同对象各自添加新行为。

比如有个Student类，我们需要获得一个学生的一年的总花费。而一个学生这一年可能需要：学费（必交）、住宿舍交宿舍费、买书交书费、交女朋友。那么它的总花费应该是这些的和，有的没有女朋友，那么花费计算就不一样了，可以像如下方式进行定义：

```
// 定制基本款
function macbook(name) {
    this.name = name;
    this.getName = function() {
        return this.name;
    };
    this.getCost = function() {
        return 13888; //
    };
}
//定义装饰器，需要提高cup
function NeedCPU(macbook) {
    var cost = macbook.getCost();
    macbook.getCost = function() {
        return cost + 2280;
    };
}
//定义装饰器，需要扩展内存
function NeedMemory(macbook) {
    var cost = macbook.getCost();
    macbook.getCost = function() {
        return cost + 1520;
    };
}
//定义装饰器，需要预先安装正版软件
function NeedSoft(macbook) {
    var cost = macbook.getCost();
    macbook.getCost = function() {
        return cost + 1298;
    };
}
```
这样就定义好了三个可选的装饰器，分别表示需要宿舍，需要书本，有女朋友。那么创建两个学生对象：

```
var xmMacBook = new macbook("小明");
NeedCPU(xmMacBook);
NeedMemory(xmMacBook);
NeedSoft(xmMacBook);
console.log(xmMacBook.getName() + "的macbook需要人民币：" + xmMacBook.getCost());

var wangMacBook = new macbook("小王");
NeedCPU(wangMacBook);
NeedMemory(wangMacBook);
console.log(wangMacBook.getName() + "的macbook需要人民币：" + wangMacBook.getCost());

// 小明的macbook需要人民币：18986
// 小王的macbook需要人民币：17688
```

## 总结

装饰者模式是为已有功能动态地添加更多功能的一种方式，把每个要装饰的功能放在单独的函数里，然后用该函数包装所要装饰的已有函数对象，因此，当需要执行特殊行为的时候，调用代码就可以根据需要有选择地、按顺序地使用装饰功能来包装对象。优点是把类（函数）的核心职责和装饰功能区分开了。


-----

> 参考引用资料
>
> 《JavaScript设计模式与开发实践》
>
> [汤姆大叔的博客——深入理解JavaScript系列](http://www.cnblogs.com/TomXu/archive/2011/12/15/2288411.html)
