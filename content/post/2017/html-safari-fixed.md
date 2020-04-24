---
title: safari浏览器fixed后，被软键盘遮盖的问题
date: 2017-03-31 18:12:19
categories:
- frontend
tags:
- safari
- html
- css
- fixed
---

safari浏览器fixed后，被软键盘遮盖的问题，暂未有更好解决方案，度娘了好多方案，发现他们并未解决此问题，下文会叙述。

## 问题描述

**测试环境：ios 10.2/10.3**

简单来说就是在html5页面中底部有个fixed的区域，如图

<!-- more -->

![safari-fix](http://og8z552x2.bkt.clouddn.com/safari-fix1.png)

在点击输入框的时候，软键盘弹出，遮盖了fixed区域，如图

![safari-fix](http://og8z552x2.bkt.clouddn.com/safari-fix2.png)

但是当你点击“完成”让软键盘收起，再次点击输入框的时候，what？一切正常了～！如图（就是要这样子的嘛，之后收起弹出软键盘都正常了，不会遮盖fixed底部区域了！）

![safari-fix](http://og8z552x2.bkt.clouddn.com/safari-fix3.png)

但是，但是，还没完，在输入框里随便输入点内容，点击“提交”，关闭软键盘，之后再次点击输入框，问题依旧～，软键盘再次遮挡fixed区域。

![safari-fix](http://og8z552x2.bkt.clouddn.com/safari-fix4.png)

如上循环，问题无法解决。

## 测试代码如下

代码很简单，但还是贴一下，方便测试，只需要复制粘贴到本机即可测试上述现象

```
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title>fixed测试页面</title>
    <meta name="viewport" content="initial-scale=1, maximum-scale=1">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black">

	<style>
		.head {
			top: 0;
			height: 50px;
			line-height: 50px;
			text-align: center;
			position: fixed;
			left: 0;
			width: 100%;
			z-index: 10;
			background-color: #99CC00;
		}

		.foot {
			bottom: 0;
			padding: 10px;
			position: fixed;
			left: 0;
			width: 100%;
			background-color: #99CC00;
		}

		.main {
			width: 100%;
			height: 100%;
			overflow-y: scroll;
			-webkit-overflow-scrolling: touch;
		}

		.list {
			padding: 0;
		}

		.list li {
			list-style: none;
			background: #ccc;
			height: 140px;
			margin-bottom: 10px;
			width: 100%;
			padding:10px;
		}
		.list a {
			border:2px solid #999;
			display: block;
			width:88%;
			position: absolute;
			height:140px;
		}
		.input {
			width: 76%;
			line-height: 30px;
			border: none;
			float: left;
			border-radius:5px;
		}
		.btn {
			float:right;
			margin-right:15px;
			border-radius:5px;
			height:34px;
			line-height: 32px;
			padding: 0 10px;
			border: none;
			background: #000;
			color: #fff;
		}
	</style>

</head>

<body>

	<header class="head">顶部固定区域</header>

	<main class="main">
		<div class="content">
			<ul class="list">
				<li><a href="#"></a></li>
				<li><a href="#"></a></li>
				<li><a href="#"></a></li>
				<li><a href="#"></a></li>
				<li><a href="#"></a></li>
				<li><a href="#"></a></li>
				<li><a href="#"></a></li>
				<li><a href="#"></a></li>
				<li><a href="#"></a></li>
				<li><a href="#"></a></li>
			</ul>
		</div>
	</main>

	<footer class="foot" id="footer-fixed">
		<button type="button" id="btn-submit" class="btn" name="button">提交</button>
		<input type="text" id="input-txt" class="input" name="" value="" placeholder="我来说两句...(200字内)">
	</footer>

</body>

</html>

```

## 尝试过的解决方案

搜索后有很多解决方法，但发现他们都没解决上述问题，

**暂时的想法，绕过fixed点击输入框后，隐藏此区域，在顶部出现更大的输入区域以让用户输入内容。**

- [IOS中弹出键盘后出现fixed失效现象的解决方案](https://segmentfault.com/a/1190000005370182#articleHeader2)
- [移动端解决fixed和input获取焦点软键盘弹出影响定位的问题](http://blog.csdn.net/kongjiea/article/details/46545351)
- [虚拟键盘与fixed带给移动端的痛](http://www.cnblogs.com/yexiaochai/p/3561939.html)
- [使用iScroll.js解决ios4下不支持position:fixed的问题](http://www.cnblogs.com/PeunZhang/archive/2013/06/14/3117589.html)


好，至此，问题描述完毕，期待有牛人更好解决方案

-----

> segmentfault 专栏提出了问题
>
> https://segmentfault.com/a/1190000008906437?_ea=1772278





