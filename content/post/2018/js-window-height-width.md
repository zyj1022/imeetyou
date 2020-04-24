---
title: 原生JS 获取浏览器、窗口、元素等尺寸的方法及注意事项
date:  2018-03-07T09:19:11+08:00
tags: ["frontend"]
categories: ["frontend"]
---

## 一、通过浏览器获得屏幕的尺寸
屏幕分辨率的高：window.screen.height
屏幕分辨率的宽：window.screen.width
屏幕可用工作区高度：window.screen.availHeight
屏幕可用工作区宽度：window.screen.availWidths

## 二、获取浏览器窗口内容的尺寸

网页可见区域宽：document.body.clientWidth
网页可见区域高：document.body.clientHeight
网页可见区域宽：document.body.offsetWidth (包括边线的宽)
网页可见区域高：document.body.offsetHeight (包括边线的宽)
网页正文全文宽：document.body.scrollWidth
网页正文全文高：document.body.scrollHeight
网页被卷去的高：document.body.scrollTop
网页被卷去的左：document.body.scrollLeft
网页正文部分上：window.screenTop
网页正文部分左：window.screenLeft

```
//高度
window.innerHeight || document.documentElement.clientHeight || document.body.clientHeight
//宽度
window.innerWidth || document.documentElement.clientWidth || document.body.clientWidht
/ *
  * window.innerHeight   FF/CH 支持，获取窗口尺寸。
  * document.documentElement.clientHeight    IE/CH支持
  * document.body.client    通过body元素获取内容的尺寸
* /
```
## 三、滚动条支持的差异性
不进行任何滚动条更改的页面，Firefox/IE 默认认为HTML元素具有滚动条属性。而body不具有。
但Chrome 则认为body是具有滚动条属性的。
因此兼容性的写法是：
```
document.documentElement.scrollTop || document.body.scrollTop
```

## 四、元素的偏移属性

```
element.offsetTop  //获取元素与其偏移参考父元素顶部的间隔距离
element.offsetLeft  //获取元素与其偏移参考父元素左边的间隔距离
element.offsetParent  //获取当前元素的参考父元素
```
- offsetTop 可以获取元素距其上一级的偏移参考父元素顶部的距离。包括：margin[top] + top
- offsetLeft 可以获取元素距其上一级的偏移参考父元素左边的距离。包括：margin[left] + left

***注意的是offsetParent在IE6/7，与IE8/FF/CH中存在兼容性问题：***
***在FF/Chrome/IE8+ ：***
- 如果当前元素有定位，则偏移参考父元素是其上一级的最近的那个定位元素。
- 如果当前元素没有定位，则默认以body为最终的参考父元素。

***在IE6/7：***
- 不论有没有定位，其偏移参考父元素都是其上一级的父元素。

***总的来说：***
- 不论是FF/Chrome还是IE，最终的参考父元素都是body元素， 因此兼容的方法就是获取当前元素到body元素的偏移位置值。

```
// 兼容性写法
function getOffestValue(elem){
    var Far = null;
    var topValue = elem.offsetTop;
    var leftValue = elem.offsetLeft;
    var offsetFar = elem.offsetParent;

    while(offsetFar){
        alert(offsetFar.tagName)
        topValue += offsetFar.offsetTop;
        leftValue += offsetFar.offsetLeft;
        Far = offsetFar;
        offsetFar = offsetFar.offsetParent;
    }
    return {'top':topValue,'left':leftValue,'Far':Far}
}
/*
* top 当前元素距离body元素顶部的距离。
* left 当前元素距离body元素左侧的距离。
* Far  返回最终的参考父元素。
*/
```
