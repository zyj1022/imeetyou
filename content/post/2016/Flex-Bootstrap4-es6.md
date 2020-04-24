---
title: 前端速查表Flex-Bootstrap4-es6
date: 2016-11-17 8:53:24
categories:
- frontend
tags:
- flex
- frontend
---

# Flexbox 属性速查表

Flex属性用多了自然记住,基本概念不再赘述不过还是简单罗列一下：

## 容器的属性有以下6个

- flex-direction属性决定主轴的方向（即项目的排列方向）
`flex-direction: row | row-reverse | column | column-reverse;`
	- row（默认值）：主轴为水平方向，起点在左端。
	- row-reverse：主轴为水平方向，起点在右端。
	- column：主轴为垂直方向，起点在上沿。
	- column-reverse：主轴为垂直方向，起点在下沿。

<!-- more -->

- flex-wrap属性定义，如果一条轴线排不下，如何换行。
	`flex-wrap: nowrap | wrap | wrap-reverse;`
	- nowrap（默认）：不换行。
	- wrap：换行，第一行在上方。
	- wrap-reverse：换行，第一行在下方。
	
- flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap。
	`flex-flow: <flex-direction> || <flex-wrap>`

- justify-content属性定义了项目在主轴上的对齐方式。
	`justify-content: flex-start | flex-end | center | space-between | space-around;`
	
	- flex-start（默认值）：左对齐
	- flex-end：右对齐
	- center： 居中
	- space-between：两端对齐，项目之间的间隔都相等。
	- space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍

- align-items属性定义项目在交叉轴上如何对齐。

	`align-items: flex-start | flex-end | center | baseline | stretch;`

	- flex-start：交叉轴的起点对齐。
	- flex-end：交叉轴的终点对齐。
	- center：交叉轴的中点对齐。
	- baseline: 项目的第一行文字的基线对齐。
	- stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。
	
- align-content属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。

	`align-content: flex-start | flex-end | center | space-between | space-around | stretch;`
	- flex-start：与交叉轴的起点对齐。
	- flex-end：与交叉轴的终点对齐。
	- center：与交叉轴的中点对齐。
	- space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。
	- space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
	- stretch（默认值）：轴线占满整个交叉轴。


## 项目的属性6个


* `order: <integer>;`  数值越小，排列越靠前，默认为0。
* `flex-grow: <number>;`  定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大
* `flex-shrink: <number>;`  定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小
* `flex-basis: <length> | auto;`  定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小
* `flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]` flex属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选。
* `align-self: auto | flex-start | flex-end | center | baseline | stretch;` align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。


属性不熟悉的可以查看阮氏教程 [http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)

下面还有问答形式的图示，方便快速查看

![flexboxsheet](http://og8z552x2.bkt.clouddn.com/flexboxsheet.png)

图片原文链接 [http://jonibologna.com/flexbox-cheatsheet/](http://jonibologna.com/flexbox-cheatsheet/)

# [Flexbox 视觉指南](https://demos.scotch.io/visual-guide-to-css3-flexbox-flexbox-playground/demos/)

这个特色就是有实时代码演示，当用户设置属性，DEMO 可以实时更新效果，这样用户更好更容易地理解 Flexbox 某个属性的作用。

# [Bootstrap 4 Cheat Sheet](https://hackerthemes.com/bootstrap-cheatsheet/)

Bootstrap 4 有新的很多组件样式，熟记它不太可能，在官网找又太麻烦，那么这个Bootstrap 4是你最好的选择，流布布局分类，支持一件复制代码，超好用，小编正在用了。

# [es6-cheatsheet](https://github.com/DrkSephy/es6-cheatsheet/blob/master/README_zhCn.md)

这是一个 ES2015(ES6) 的Cheatsheet，其中包括提示、小技巧、最佳实践和一些代码片段，帮助你 完成日复一日的开发工作。