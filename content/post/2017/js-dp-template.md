---
title: 设计模式之模板模式(TemplateMethod)
date: 2017-04-06T17:54:16+08:00
categories:
- frontend
tags:
- 设计模式
- js
---

在 JavaScript 开发中用到继承的场景其实并不是很多，很多时候我们都喜欢用 mix-in 的方式给对象扩展属性。但这不代表继承在 JavaScript 里没有用武之地，虽然没有真正的类和继承机制，但我们可以通过原型 prototype 来变相地实现继承。

## 定义

模板方法模式是一种只需使用继承就可以实现的非常简单的模式。

**模板方法模式由两部分结构组成：**

- 第一部分是抽象父类，
- 第二部分是具体的实现子类。

通常在抽象父类中封装了子类的算法框架，包括实现一些公共方法以及封装子类中所有方法的执行顺序。子类通过继承这个抽象类，也继承了整个算法结构，并且可以选择重写父类的方法。

<!-- more -->

## 使用实例

### Coffee or Tea

咖啡与茶是一个经典的例子，经常用来讲解模板方法模式

泡茶和泡咖啡有同样的步骤，比如烧开水（boilWater）、冲泡（brew）、倒在杯子里（pourInCup），加小料（addCondiments）等等。

但每种饮料冲泡的方法以及所加的小料不一样，所以我们可以利用模板方法实现这个主要步骤

首先先来定义抽象步骤：

```
var Beverage = function(){};
Beverage.prototype.boilWater = function(){
	 console.log( '把水煮沸' );
};
Beverage.prototype.brew = function(){}; // 空方法，应该由子类重写
Beverage.prototype.pourInCup = function(){}; // 空方法，应该由子类重写
Beverage.prototype.addCondiments = function(){}; // 空方法，应该由子类重写
Beverage.prototype.init = function(){
	this.boilWater();
	this.brew();
	this.pourInCup();
	this.addCondiments();
};
```

接下来我们要创建咖啡类和茶类，并让它们继承抽象类:

```
// 冲咖啡
var Coffee = function(){};
Coffee.prototype = new Beverage();
Coffee.prototype.brew = function() {
	console.log( '用沸水冲泡咖啡' );
}
Coffee.prototype.pourInCup = function() {
	console.log( '把咖啡倒进杯子' );
}
Coffee.prototype.addCondiments = function() {
	console.log( '加糖和牛奶' );
}
var Coffee = new Coffee();
Coffee.init();

// 冲茶叶
var Tea = function(){};
Tea.prototype = new Beverage();
Tea.prototype.brew = function() {
	console.log( '用沸水冲泡茶叶' );
}
Tea.prototype.pourInCup = function() {
	console.log( '把茶倒进杯子' );
}
Tea.prototype.addCondiments = function() {
	console.log( '加柠檬' );
}
var Tea = new Tea();
Tea.init();
```

那么在上面的例子中，到底谁才是所谓的模板方法呢?答案是 `Beverage.prototype.init`。

`Beverage.prototype.init` 被称为模板方法的原因是，该方法中封装了子类的算法框架，它作为一个算法的模板，指导子类以何种顺序去执行哪些方法。在 `Beverage.prototype.init` 方法中，算法内的每一个步骤都清楚地展示在我们眼前。


## 好莱坞原则

我们要引入一个新的设计原则——著名的“好莱坞原则”。

好莱坞原则简单来说就是：“别找我们，我们来找你”，是一种反向的控制结构，指的是父类调用一个类的操作。

在这一原则的指导下，模板方法模式就是允许底层组件将自己挂钩到高层组件中，而高层组件会决定什么时候、以何种方式去使用这些底层组件。

在好莱坞原则的指导之下，下面这段代码可以达到和使用继承一样的效果：

```
var Beverage = function(param) {

	var boilWater = function() {
		console.log( '0 把水煮沸' );
	};

	var brew = param.brew || function() {
		throw new Error( '必须传递 brew 方法' );
	};

	var pourInCup = param.pourInCup || function() {
		throw new Error( '必须传递 pourInCup 方法' );
	};

	var addCondiments = param.addCondiments || function() {
		throw new Error( '必须传递 addCondiments 方法' );
	};

	var F = function(){};

	F.prototype.init = function(){
		boilWater();
	    brew();
		pourInCup();
		addCondiments();
	};

	return F;
};

var Coffee = Beverage({
	brew: function() {
		console.log( '1 用沸水冲泡咖啡' );
	},
	pourInCup: function() {
		console.log( '2 把咖啡倒进杯子' );
	},
	addCondiments: function() {
		console.log( '3 加糖和牛奶' );
	}
});

var coffee = new Coffee();
coffee.init();

var Tea = Beverage({
	brew: function() {
		console.log( '1 用沸水浸泡茶叶' );
	},
	pourInCup: function(){
		console.log( '2 把茶倒进杯子' );
	},
	addCondiments: function() {
		console.log( '3 加柠檬' );
	}
});

var tea = new Tea();
tea.init();
```


## 总结

模板方法应用于下列情况：

- 一次性实现一个算法的不变的部分，并将可变的行为留给子类来实现
- 各子类中公共的行为应被提取出来并集中到一个公共父类中的避免代码重复，不同之处分离为新的操作，最后，用一个钓鱼这些新操作的模板方法来替换这些不同的代码
- 控制子类扩展，模板方法只在特定点调用“hook”操作，这样就允许在这些点进行扩展

和策略模式不同，模板方法使用继承来改变算法的一部分，而策略模式使用委托来改变整个算法。

-----

> 参考引用资料
>
> 《JavaScript设计模式与开发实践》
>
> [汤姆大叔的博客——深入理解JavaScript系列](http://www.cnblogs.com/TomXu/archive/2011/12/15/2288411.html)
