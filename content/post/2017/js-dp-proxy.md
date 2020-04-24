---
title: 设计模式之代理模式（Proxy）
date: 2017-03-29 18:16:09
categories:
- frontend
tags:
- 设计模式
- js
---

代理模式是为一个对象提供一个代用品或占位符，以便控制对它的访问。

代理模式的关键是，当客户不方便直接访问一个对象或者不满足需要的时候，提供一个替身对象来控制对这个对象的访问，客户实际上访问的是替身对象。替身对象对请求做出一些处理之后，再把请求转交给本体对象。

## 定义

代理，顾名思义就是帮助别人做事，GoF对代理模式的定义如下：

> 代理模式（Proxy），为其他对象提供一种代理以控制对这个对象的访问。

<!--more-->

## 使用实例


### 简单实例

我们来举一个简单的例子，假如小明要送酸奶小妹玫瑰花，却不知道她的联系方式或者不好意思，想委托大叔去送这些玫瑰，那大叔就是个代理，那我们如何来做呢？

```
// 先声明美女对象
var girl = function (name) {
    this.name = name;
};

// 这是小明
var xiaoMing = function (girl) {
    this.girl = girl;
    this.sendGift = function (gift) {
        alert("Hi " + girl.name + ", 小明送你一个礼物：" + gift);
    }
};

// 大叔是代理
var proxyTom = function (girl) {
    this.girl = girl;
    this.sendGift = function (gift) {
        (new xiaoMing(girl)).sendGift(gift); // 替dudu送花咯
    }
};
```

调用方式：

```
var proxy = new proxyTom(new girl("酸奶小妹"));
proxy.sendGift("999朵玫瑰");
```

### 虚拟代理实现图片预加载

在 Web 开发中，图片预加载是一种常用的技术，如果直接给某个 img 标签节点设置 src 属性， 由于图片过大或者网络不佳，图片的位置往往有段时间会是一片空白。常见的做法是先用一张 loading 图片占位，然后用异步的方式加载图片，等图片加载好了再把它填充到 img 节点里，这种 场景就很适合使用虚拟代理。

下面我们来实现这个虚拟代理，首先创建一个普通的本体对象，这个对象负责往页面中创建 一个 img 标签，并且提供一个对外的 setSrc 接口，外界调用这个接口，便可以给该 img 标签设置
src 属性:

```
var myImage = (function(){
	var imgNode = document.createElement( 'img' );
	document.body.appendChild( imgNode );
	return {
		setSrc: function( src ) {
			imgNode.src = src;
		}
	}
})();
```
我们把网速调至 5KB/s，然后通过 MyImage.setSrc 给该 img 节点设置 src，可以看到，在图片

被加载好之前，页面中有一段长长的空白时间。

现在开始引入代理对象 proxyImage，通过这个代理对象，在图片被真正加载好之前，页面中
将出现一张占位的菊花图 loading.gif, 来提示用户图片正在加载。代码如下:

```
var myImage = (function(){
	var imgNode = document.createElement( 'img' );
	document.body.appendChild( imgNode );
	return {
		setSrc: function( src ) {
			imgNode.src = src;
		}
	}
})();

var proxyImage = (function() {
	var img = new Image;
	img.onload = function() {
		myImage.setSrc( this.src );
	}
	return {
		setSrc: function( src ) {
			myImage.setSrc( './pic.jpg' );
			img.src = src;
		}
	}
})();

proxyImage.setSrc( './loading.gif' );
```

## 总结

在 JavaScript 开发中最常用的是虚拟代理和缓存代理。虽然代理模式非常有用，但我们在编写业务代码的时候，往往不需要去预先猜测是否需要使用代理模式。 当真正发现不方便直接访问某个对象的时候，再编写代理也不迟。


-----

> 参考引用资料
>
> [汤姆大叔的博客——深入理解JavaScript系列](http://www.cnblogs.com/TomXu/archive/2011/12/15/2288411.html)
>
