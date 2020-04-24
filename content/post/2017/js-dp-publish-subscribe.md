---
title: 设计模式之发布/订阅模式(Publish/Subscribe)
date: 2017-04-05 12:36:29
categories:
- frontend
tags:
- 设计模式
- js
---

发布/订阅模式又叫观察者模式，它定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都将得到通知。在 JavaScript 开发中，我们一般用事件模型来替代传统的发布/订阅模式

## 遍地的发布订阅现象

如今的信息化时代，发布/订阅模式的应用可以说非常广泛，比如微信公众号就是典型的发布/订阅模式，公众号发布一条信息，所有的订阅者都会收到。

有人可能也会想到经常收到的各种广告短信信息（有的可能是被动订阅），其实发送短信通知或广告也是一个典型的发布/订阅模式。

发布/订阅模式可以广泛用于异步编程中，代替传递回掉函数的方案，比如，我们可以订阅 ajax 请求的 error、succ 等事件。

另外发表订阅让两个对象松耦合在一起，不必了解彼此细节，当有新的订阅者出现时，发布者的代码不需要任何修改。同样发布者需要改变时，也不会影响到之前的订阅者。只要之前约定的事件名没有变化，就可以自由地改变它们。

<!-- more -->


## 定义

发布订阅模式，它定义了一种一对多的关系，让多个观察者对象同时监听某一个主题对象，这个主题对象的状态发生变化时就会通知所有的观察者对象，使得它们能够自动更新自己。

使用发布订阅模式的好处：

* 支持简单的广播通信，自动通知所有已经订阅过的对象。
* 页面载入后目标对象很容易与观察者存在一种动态关联，增加了灵活性。
* 目标对象与观察者之间的抽象耦合关系能够单独扩展以及重用。


## 使用实例

### 自定义发表订阅

我们来尝试一个自定义的发布订阅模式，那么如何实现发布订阅呢

- 指定一个发布者
- 给发布者添加一个缓冲列表，用于存放回调函数以用于通知订阅者
- 发布消息时，发布者遍历缓存列表，依次触发每个订阅者的回调函数

**一个简单的天气状态订阅**

```
var Weather = {
	list: [], // 缓存列表
	listen: function(fn) { // 增加订阅者
		this.list.push(fn)
	},
	publish: function() { // 发布消息
		for(var i=0,fn; fn=this.list[i++];) {
			fn.apply(this,arguments);
		}
	}
};

// 订阅消息
Weather.listen(function(weather, wind){
	console.log('天气：' + weather, '风力：'+ wind);
})

// 发布消息
Weather.publish("晴天","微风"); // 天气：晴天 风力：微风
Weather.publish("雷阵雨","5级风"); // 天气：雷阵雨 风力：5级风
```
以上，已经实现了一个最简单的发布—订阅模式，还可以为订阅者增加自选功能，订阅自己想要的消息，也可以增加取消订阅的事件。

```
var PubSub = {
	list: [],
	listen: function(key, fn){
		if(!this.list[key]) {
			this.list[key]=[];
		}
		this.list[key].push(fn);
	},
	publish: function(){
		var key = Array.prototype.shift.call(arguments),
		    fns = this.list[key];
		if(!fns || fns.length === 0) {
			return false;
		}
		for(var i = 0, fn; fn = fns[i++];){
			fn.apply(this, arguments);
		}
	}
}

//
var installEvent = function(obj) {
	for (var i in PubSub) {
		obj[i] = PubSub[i];
	}
};

var day = {}
installEvent(day);

day.listen('天气', function(wind) {
	console.log('风力：'+ wind);
});

day.publish('天气', "8级风");
```

### 实战之网站登录

网站登录是最常见的形式，通常在登录以后我们会ajax异步请求获取用户信息，比如显示用户名字、头像等信息在header模块，而这两个字段都是来自用户登录后返回的信息。至于 ajax 请求什么时候能成功返回用户信息，这点我们没有办法确定，虽然现在看起来和发布订阅模式没关系，因为异步的问题通常也可以回调函数来解决。

我们不知道除了 header 头部、nav 导航、消息列表、购物车之外，将来还有哪些模块需要使用这些用户信息。如果它们和用户信息模块产生了强耦合，比如下面这样的形式:

```
login.succ(function(data){
	header.setAvatar( data.avatar);  // 设置 header 模块的头像
	nav.setAvatar( data.avatar ); // 设置导航模块的头像
	message.refresh(); // 刷新消息列表
	cart.refresh(); // 刷新购物车列表
});
```
现在登录模块是我们负责编写的，但我们还必须了解 header 模块里设置头像的方法叫 setAvatar、购物车模块里刷新的方法叫 refresh，这种耦合性会使程序变得僵硬，header 模块不能随意再改变 setAvatar 的方法名，它自身的名字也不能被改为 header1、header2。 这是针对具 体实现编程的典型例子，针对具体实现编程是不被赞同的。

某一个，项目新增加收获地址管理模块：

```
login.succ(function(data){
	header.setAvatar( data.avatar);
	nav.setAvatar( data.avatar );
	message.refresh();
	address.refresh(); // 新增加收获地址
});
```
现在我们用发布订阅重写，对用户信息感兴趣的业务模块将自行订阅登录成功的消息事件。 当登录成功时，登录模块只需要发布登录成功的消息，而业务方接受到消息之后，就会开始进行各自的业务处理，登录模块并不关心业务方究竟要做什么，也不想去了解它们的内部细节。改善后的代码如下:

```
$.ajax( 'http://xxx.com?login', function(data){ // 登录成功
	login.trigger( 'loginSucc', data); // 发布登录成功的消息
});
```

各模块监听登录成功的消息:

```
var header = (function() { // header 模块
	login.listen( 'loginSucc', function(data) {
        header.setAvatar( data.avatar );
	});
	return {
		setAvatar: function(data) {
			console.log( '设置 header 模块的头像');
		}
	}
})();

var nav = (function() {  // nav 模块
	login.listen('loginSucc', function(data) {
		nav.setAvatar( data.avatar );
	});
	return {
		setAvatar: function(avatar) {
			console.log( '设置 nav 模块的头像');
		}
	}
})();

```

如果有一天在登录完成之 后，又增加一个刷新收货地址列表的行为，那么只要在收货地址模块里加上监听消息的方法即可，而这可以让开发该模块的同事自己完成，你作为登录模块的开发者，永远不用再关心这些行为了

```
var address = (function(){ // 收获地址模块
	login.listen('loginSucc', function(obj){
        address.refresh(obj);
    });
	return {
		refresh: function( avatar ){
			console.log( '刷新收货地址列表' );
		}
	}
})();
```

## 总结

发布订阅的使用场合就是：当一个对象的改变需要同时改变其它对象，并且它不知道具体有多少对象需要改变的时候，就应该考虑使用观察者模式。

总的来说，发布订阅模式所做的工作就是在解耦，让耦合的双方都依赖于抽象，而不是依赖于具体。从而使得各自的变化都不会影响到另一边的变化。

另外， 发布—订阅模式虽然可以弱化对象之间的联系，但如果过度使用的话，对象和对象之间的必要联系也将被深埋在背后，会导致程序难以跟踪维护和理解。特别是有多个发布者和订阅者嵌套到一起的时候，要跟踪一个 bug 不是件轻松的事情。

-----

> 参考引用资料
>
> 《JavaScript设计模式与开发实践》
>
> [汤姆大叔的博客——深入理解JavaScript系列](http://www.cnblogs.com/TomXu/archive/2011/12/15/2288411.html)
