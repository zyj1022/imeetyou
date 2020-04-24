---
title: 设计模式之中介者模式(Mediator)
date: 2017-04-19 12:36:29
categories:
- frontend
tags:
- 设计模式
- js
---

## 介绍

中介者模式的作用就是解除对象与对象之间的紧耦合关系。增加一个中介者对象后，所有的相关对象都通过中介者对象来通信，而不是互相引用，所以当一个对象发生改变时，只需要通知中介者对象即可。中介者使各对象之间耦合松散，而且可以独立地改变它们之间的交互。中介者模式使网状的多对多关系变成了相对简单的一对多关系.

<!-- more -->

![中介者模式](http://og8z552x2.bkt.clouddn.com/js-mediator.png)


## 使用实例

开发中，中介者是一个行为设计模式，通过提供一个统一的接口让系统的不同部分进行通信。一般，如果系统有很多子模块需要直接沟通，都要创建一个中央控制点让其各模块通过该中央控制点进行交互。中介者模式可以让这些子模块不需要直接沟通，而达到进行解耦的目的。

打个比方，平时常见的机场交通控制系统，塔台就是中介者，它控制着飞机（子模块）的起飞和降落，因为所有的沟通都是从飞机向塔台汇报来完成的，而不是飞机之前相互沟通。中央控制系统就是该系统的关键，也就是软件设计中扮演的中介者角色。

## 实例

### 购买商品筛选

假设我们正在编写一个手机购买的页面，在购买流程中，

- 选择手机颜色、购买数量（如果购买数量大于库存则显示库存不足，禁用下一步按钮）
- 下一步按钮，加入购物车

可能想到的扩展，购买过程中不仅选择筛选颜色，还会加入内存、品牌、价格区间等等。

为了减少耦合，筛选条件之间彼此独立，引入中介者模式，当其中某个条件发生改变时，仅仅通知中介者它被改变了， 同时把自身当作参数传入中介者，以便中介者辨别是谁发生了改变。剩下的所有事情都交给中介者对象来完成，这样一来，无论是修改还是新增节点，都只需要改动中介者对象里的代码。

```
var goods = {  // 手机库存
	"red|32G": 3,
	"red|16G": 0,
	"blue|32G": 1,
	"blue|16G": 6
}

var mediator = (function() {
	var colorSelect = document.getElementById( 'colorSelect' ),
		memorySelect = document.getElementById( 'memorySelect' ),
		numberInput = document.getElementById( 'numberInput' ),
		colorInfo = document.getElementById( 'colorInfo' ),
		memoryInfo = document.getElementById( 'memoryInfo' ),
		numberInfo = document.getElementById( 'numberInfo' ),
		nextBtn = document.getElementById( 'nextBtn' );
	return {
		changed: function( obj ){
			var color = colorSelect.value, // 颜色
			memory = memorySelect.value,// 内存
			number = numberInput.value, // 数量
			stock = goods[ color + '|' + memory ];
			// 颜色和内存对应的手机库存数量
			if ( obj === colorSelect ){ // 如果改变的是选择颜色下拉框
				colorInfo.innerHTML = color;
			} else if ( obj === memorySelect ) {
				memoryInfo.innerHTML = memory;
			} else if ( obj === numberInput ) {
				numberInfo.innerHTML = number;
			}
			if ( !color ) {
				nextBtn.disabled = true;
				nextBtn.innerHTML = '请选择手机颜色';
				return;
			}
			if ( !memory ) {
				nextBtn.disabled = true;
				nextBtn.innerHTML = '请选择内存大小';
				return;
			}
			if ( ( ( number - 0 ) | 0 ) !== number - 0 ){
				nextBtn.disabled = true;
				nextBtn.innerHTML = '请输入正确的购买数量';
				return;
			}
			nextBtn.disabled = false;
			nextBtn.innerHTML = '放入购物车';
		}
	}
})();

// 事件函数:
colorSelect.onchange = function(){
	mediator.changed( this );
};
memorySelect.onchange = function(){
	mediator.changed( this );
};
numberInput.oninput = function(){
	mediator.changed( this );
};
```

以上，如果某一天又要新增一些筛选选择条件，我们只需要修改 mediator 对象即可。


## 总结

中介者模式是迎合迪米特法则的一种实现。迪米特法则也叫最少知识原则，是指一个对象应该尽可能少地了解另外的对象(类似不和陌生人说话)。如果对象之间的耦合性太高，一个对象发生改变之后，难免会影响到其他的对象，跟“城门失火，殃及池鱼”的道理是一样的。而在中介者模式里，对象之间几乎不知道彼此的存在，它们只能通过中介者对象来互相影响对方。


-----

> 参考引用资料
>
> 《JavaScript设计模式与开发实践》
>
> [汤姆大叔的博客——深入理解JavaScript系列](http://www.cnblogs.com/TomXu/archive/2011/12/15/2288411.html)
