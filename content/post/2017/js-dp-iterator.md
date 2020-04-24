---
title: 设计模式之迭代器模式(Iterator)
date: 2017-03-31 17:56:19
categories:
- frontend
tags:
- 设计模式
- Iterator
- js
---

迭代器模式是一种相对简单的模式，简单到很多时候我们都不认为它是一种设计模式。目前的绝大部分语言都内置了迭代器。

比如：JavaScript 的 `Array.prototype.forEach`

jQuery里一个非常有名的迭代器就是 `$.each` 方法，通过each我们可以传入额外的function，然后来对所有的item项进行迭代操作，例如：

```
$.each( [1, 2, 3], function( i, n ){
	console.log( '当前下标为： '+ i,'当前值为:' + n );
});

// 下标： 0 当前值:1
// 下标： 1 当前值:2
// 下标： 2 当前值:3
```
<!--more-->

## 定义

迭代器模式(Iterator)：提供一种方法顺序一个聚合对象中各个元素，而又不暴露该对象内部表示。

迭代器的几个特点是：

- 访问一个聚合对象的内容而无需暴露它的内部表示。
- 为遍历不同的集合结构提供一个统一的接口，从而支持同样的算法在不同的集合结构上进行操作。
- 遍历的同时更改迭代器所在的集合结构可能会导致问题（比如C#的foreach里不允许修改item）。


## 使用实例

### 实现自己的迭代器

参照jQuery的`$.each` 方法, 我们来自己实现一个 each 函数，each 函数接受 2 个参数，第一个为被循环的数组，第二个为循环中的每一步后将被触发的回调函数:

```
var each = function( ary, callback ) {
	for ( var i = 0, l = ary.length; i < l; i++ ){
		callback.call( ary[i], i, ary[ i ] );
		// 把下标和元素当作参数传给callback 函数
	}
};

each( [ "a", "b", "c" ], function( i, n ){
	console.log( '自定义下标为： '+ i,'自定义值为:' + n );
});

// 自定义下标为： 0 自定义值为:a
// 自定义下标为： 1 自定义值为:b
// 自定义下标为： 2 自定义值为:c
```

### 迭代器模式的应用举例

以常用的上传文件功能为例，在不同的浏览器环境下，选择的上传方式是不一样的。因为使用浏览器的上传控件进行上传速度快，可以暂停和续传，所以我们首先会优先使用控件上传。如果浏览器没有安装上传控件， 则使用 Flash 上传， 如果连 Flash 也没安装，那就只好使用浏览器原生的表单上传了，代码为：

```
var getUploadObj = function() {
	try {
		return new ActiveXObject("TXFTNActiveX.FTNUpload"); // IE 上传控件
	} catch (e) {
		if (supportFlash()) { // supportFlash 函数未提供
			var str = '<object type="application/x-shockwave-flash"></object>';
			return $(str).appendTo($('body'));
		} else {
			var str = '<input name="file" type="file"/>'; // 表单上传
			return $(str).appendTo($('body'));
		}
	}
};
```
看看上面的代码，为了得到一个 upload 对象，这个 getUploadObj 函数里面充斥了 try，catch 以及 if 条件分支。缺点是显而易见的。

现在来梳理一下问题，目前一共有 3 种可能的上传方式，我们不知道目前浏览器支持那种上传方式，那就需要逐个尝试，直到成功为止，分别定义以下几个函数：


```
// IE 上传控件
var getActiveUploadObj = function() {
	try {
		return new ActiveXObject("TXFTNActiveX.FTNUpload");
	} catch (e) {
		return false;
	}
};

// Flash 上传
var getFlashUploadObj = function() {
	if (supportFlash()) { // supportFlash 函数未提供
		var str = '<object type="application/x-shockwave-flash"></object>';
		return $(str).appendTo($('body'));
	};
	return false;
}

// 表单上传
var getFormUpladObj = function() {
	var str = '<input name="file" type="file" class="ui-file"/>';
	return $(str).appendTo($('body'));
};
```

在以上三个函数中，如果该函数里面的 upload 对象是可用的，则让函数返回该对象，反之返回 false，提示迭代器继续往后面进行迭代。

那么迭代器只需进行下面这几步工作：

- 提供一个可以被迭代的方法，使得 getActiveUploadObj，getFlashUploadObj 以及 getFlashUploadObj 依照优先级被循环迭代。
- 如果正在被迭代的函数返回一个对象，则表示找到了正确的 upload 对象，反之如果该函数返回 false，则让迭代器继续工作。

```
var iteratorUpload = function() {
	for (var i = 0, fn; fn = arguments[i++];) {
		var upload = fn();
		if (upload !== false) {
			return upload;
		}
	}
};

var uploadObj = iteratorUpload( getActiveUploadObj, getFlashUploadObj, getFormUpladObj )
```

重构代码之后，可以看到，上传对象的各个函数彼此分离互补干扰，很方便维护和扩展，如果后期增加了 Webkit 控件上传和 HTML5 上传，我们就增加对应的函数功能并在迭代器里添加：

```
var getWebkitUploadObj = function(){
 // 具体代码略
}

var getHtml5UploadObj = function(){
 // 具体代码略
}

// 依照优先级把它们添加进迭代器
var uploadObj = iteratorUploadObj( getActiveUploadObj, getWebkitUploadObj,
getFlashUploadObj, getHtml5UploadObj, getFormUpladObj );

```

## 总结

迭代器的使用场景是：对于集合内部结果常常变化各异，我们不想暴露其内部结构的话，但又想让客户代码透明地访问其中的元素，这种情况下我们可以使用迭代器模式。

-----

> 参考引用资料
>
> 《JavaScript设计模式与开发实践》
>
> [汤姆大叔的博客——深入理解JavaScript系列](http://www.cnblogs.com/TomXu/archive/2011/12/15/2288411.html)
>
