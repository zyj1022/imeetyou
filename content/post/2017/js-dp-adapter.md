---
title: 设计模式之适配器模式(Adapter)
date: 2017-04-24 17:26:19
categories:
- frontend
tags:
- 设计模式
- js
---

## 介绍

适配器模式（Adapter）是将一个类（对象）的接口（方法或属性）转化成客户希望的另外一个接口（方法或属性），适配器模式使得原本由于接口不兼容而不能一起工作的那些类（对象）可以一些工作。

适配器的别名是包装器(wrapper)，这是一个相对简单的模式。

## 使用实例

### 现实中的适配器——apple的各种转接头

相信大家对apple对各种设备设计精良，用户使用方便比较赞同，但是针对各种转接头又有各种吐槽，其实转接头是不是就对应我们但适配器呢？

<!--more-->

如果现有的接口已经能够正常工作，那我们就永远不会用上适配器模式。适配器模式是一种 “亡羊补牢”的模式，没有人会在程序的设计之初就使用它。因为没有人可以完全预料到未来的事情，也许现在好好工作的接口，未来的某天却不再适用于新系统，那么我们可以用适配器模式 把旧接口包装成一个新的接口，使它继续保持生命力。比较典型但就是最新但 iphone7 去掉了耳机接口，提供了额外但转接头。也许以后充电口也会去掉，用无线充电，并且提供额外但转接设备。


### 地图调用

当我们向 googleMap 和 baiduMap 都发出“显示”请求时，googleMap 和 baiduMap 分别以各自的方式在页面中展现了地图:

```
var googleMap = {
    show: function() {
        console.log( '开始渲染谷歌地图' );
    }
};
var baiduMap = {
    show: function(){
        console.log( '开始渲染百度地图' );
    }
};
var renderMap = function( map ) {
    if ( map.show instanceof Function ){
        map.show();
    }
};

renderMap( googleMap ); // 输出:开始渲染谷歌地图
renderMap( baiduMap ); // 输出:开始渲染百度地图
```
因为 googleMap 和 baiduMap 提供了一致的 show 方法，但第三方的接口方法并不在我们自己的控制范围之内，假如 baiduMap 提供的显示地图的方法不叫 show 而叫 display 呢? baiduMap 这个对象来源于第三方，正常情况下我们都没办法改动它。此时我们可以通过增加 baiduMapAdapter 来解决问题:

```
var googleMap = {
    show: function() {
        console.log( '开始渲染谷歌地图' );
    }
};
var baiduMap = {
    display: function(){
        console.log( '开始渲染百度地图' );
    }
};

var baiduMapAdapter = {
    show: function(){
        return baiduMap.display();
    }
}

var renderMap = function( map ) {
    if ( map.show instanceof Function ){
        map.show();
    }
};


renderMap( googleMap ); // 输出:开始渲染谷歌地图
renderMap( baiduMapAdapter );; // 输出:开始渲染百度地图
```

### 动物叫声

鸭子但叫声为 quack, 而公鸡但叫声为 crow, 假如我们也要公鸡实现嘎嘎叫（quack）这个动作。我们就可以复用鸭子但quack方法，但发出但声音还是 crow 的。

首先要先定义鸭子和火鸡的抽象行为，也就是各自的方法函数：

```
var Duck = function(){ }
Duck.prototype.quack =function() {
    throw new Error(" 该方法必须被重写 !");
}

var Cock = function(){ }
Cock.prototype.crow = function() {
    throw new Error(" 该方法必须被重写 !");
}
```

然后再定义具体的鸭子和公鸡的构造函数，分别为：

```
var MallarDuck = function() {
    Duck.apply(this);
}
MallarDuck.prototype = new Duck();
MallarDuck.prototype.quack = function() {
    console.log("嘎嘎！嘎嘎！");
}

var WildCock = function() {
    Cock.apply(this);
}
WildCock.prototype = new Cock();
WildCock.prototype.crow = function() {
    console.log("咯咯！咯咯！");
}
```

为了让公鸡也支持quack方法，我们创建了一个新的公鸡适配器 CockAdapter

```
var CockAdapter = function(ck) {
    Duck.apply(this);
    this.ck = ck;
}
CockAdapter.prototype = new Duck();
CockAdapter.prototype.quack = function() {
    this.ck.crow();
}
```

该构造函数接受一个公鸡的实例对象，然后使用 Duck 进行 apply，其适配器原型是 Duck，然后要重新修改其原型的quack方法，以便内部调用 ck.crow() 方法。

测试一下便可以知道结果了:

```
var oDuck = new MallarDuck()
var oCock = new WildCock()
var oCkadapter = new CockAdapter(oCock);

// 原有的鸭子行为
oDuck.quack();  // 嘎嘎！嘎嘎！
// 原有但公鸡行为
oCock.crow(); // 咯咯！咯咯！

// 适配器公鸡的行为
oCkadapter.quack(); // 咯咯！咯咯！

```

## 总结

那合适使用适配器模式好呢？如果有以下情况出现时，建议使用：

- 1、使用一个已经存在的对象，但其方法或属性接口不符合你的要求；
- 2、你想创建一个可复用的对象，该对象可以与其它不相关的对象或不可见对象（即接口方法或属性不兼容的对象）协同工作；
- 3、想使用已经存在的对象，但是不能对每一个都进行原型继承以匹配它的接口。对象适配器可以适配它的父对象接口方法或属性。

另外，适配器模式和其它几个模式可能容易让人迷惑，这里说一下大概的区别：

- **适配器模式**主要用来解决两个已有接口之间不匹配的问题，它不考虑这些接口是怎样实现的，也不考虑它们将来可能会如何演化。适配器模式不需要改变已有的接口，就能够使它们协同作用。
- **装饰者模式和代理模式**也不会改变原有对象的接口，但装饰者模式的作用是为了给对象**增加功能**。装饰者模式常常形成一条长的装饰链，而**适配器模式通常只包装一次**。**代理模式是为了控制对对象的访问**，通常也只包装一次。
- **外观模式**的作用倒是和适配器比较相似，有人把外观模式看成一组对象的适配器，但外观模式最显著的特点是定义了一个新的接口。

-----

> 参考引用资料
>
> 《JavaScript设计模式与开发实践》
>
> [汤姆大叔的博客——深入理解JavaScript系列](http://www.cnblogs.com/TomXu/archive/2011/12/15/2288411.html)
>
