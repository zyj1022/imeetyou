---
title: 设计模式之单例模式(Singleton)
date: 2017-03-29 11:36:29
categories:
- frontend
tags:
- 设计模式
- singleton
- js
---

在 JavaScript 开发中，单例模式的用途非常广泛。比如，当我们单击登录按钮的时候，页面中会出现一个登录浮窗，而这个登录浮窗是唯一的，无论单击多少次登录按钮，这个浮窗都只会被创建一次，那么这个登录浮窗就适合用单例模式来创建。

## 定义

单例模式的定义是：**保证一个类仅有一个实例，并提供一个访问它的全局访问点。**

## 实现方法

经典Singleton模式的实现方式是，如果实例不存在，通过一个方法创建一个实例。如果已经存在，则返回实例的引用。

Singleton与静态类（对象）不同的是，它可以被延迟生成，只有在需要的时候才会生成实例。

在JavaScript里，实现单例的方式有很多种，其中最简单的一个方式是使用对象字面量的方法，其字面量里可以包含大量的属性和方法：

<!--- more -->

```
var singleton = (function() {
	var instance;

	function init() {
		// define private methods and properties
		// do something
		return {
			// define public methods and properties
			publicMethod: function() {
				console.log('hello singleton!')
			},
			publicProperty: 'test'
		};
	}
	return {
		getInstance: function() {
			if (!instance) {
				instance = init()
			}
			return instance
		}
	}
})();

singleton.getInstance().publicMethod(); // hello singleton!
console.log(singleton.getInstance().publicProperty); // test
```

## 其它实现方法

### 方法1：

```
function Universe() {

    // 判断是否存在实例
    if (typeof Universe.instance === 'object') {
        return Universe.instance;
    }

    // 其它内容
    this.start_time = 0;
    this.bang = "Big";

    // 缓存
    Universe.instance = this;

    // 隐式返回this
}

// 测试
var uni = new Universe();
var uni2 = new Universe();
console.log(uni === uni2); // true
````

### 方法2：

```
function Universe() {

    // 缓存的实例
    var instance = this;

    // 其它内容
    this.start_time = 0;
    this.bang = "Big";

    // 重写构造函数
    Universe = function () {
        return instance;
    };
}

// 测试
var uni = new Universe();
var uni2 = new Universe();
uni.bang = "123";
console.log(uni === uni2); // true
console.log(uni2.bang); // 123
````
### 方法3：

```
function Universe() {

    // 缓存实例
    var instance;

    // 重新构造函数
    Universe = function Universe() {
        return instance;
    };

    // 后期处理原型属性
    Universe.prototype = this;

    // 实例
    instance = new Universe();

    // 重设构造函数指针
    instance.constructor = Universe;

    // 其它功能
    instance.start_time = 0;
    instance.bang = "Big";

    return instance;
}

// 测试
var uni = new Universe();
var uni2 = new Universe();
console.log(uni === uni2); // true

// 添加原型属性
Universe.prototype.nothing = true;

var uni = new Universe();

Universe.prototype.everything = true;

var uni2 = new Universe();

console.log(uni.nothing); // true
console.log(uni2.nothing); // true
console.log(uni.everything); // true
console.log(uni2.everything); // true
console.log(uni.constructor === Universe); // true
```

### 方式4:

```
var Universe;

(function () {

    var instance;

    Universe = function Universe() {

        if (instance) {
            return instance;
        }

        instance = this;

        // 其它内容
        this.start_time = 0;
        this.bang = "Big";
    };
} ());

//测试代码
var a = new Universe();
var b = new Universe();
alert(a === b); // true
a.bang = "123";
alert(b.bang); // 123
```

----

> 参考及引用资料：
>
> https://github.com/shichuan/javascript-patterns/blob/master/design-patterns/singleton.html
