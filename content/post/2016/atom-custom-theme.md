---
title: 如何自定义Atom主题
date: 2016-12-08 12:42:32
categories:
- frontend
tags:
- atom
- theme
- syntax
---

使用Atom有段时间，一直使用别人的主题，想着自己也制作一款主题，便再 [官网](https://atom.io/docs/v1.2.4/hacking-atom-creating-a-theme) 上研究一番：


aotm主题，分为两种，一种是界面主题，一种是语法主题，下面就一一试试。



# 创建界面主题

首先，去下载一个主题，一直使用的是这个 [one-dark-ui](https://atom.io/themes/one-dark-ui),下载到本机，并修改文件及主题名称，我修改的 `codyer-theme-ui`,然后，执行链接命令

```
cd ~/.atom/packages
apm link /Users/username/Desktop/codyer-theme-ui
```

之后，打开设置，选择 themes，就可以看到了。

<!-- more -->

## 即时重启

在你修改你的主题之后，按下 `cmd-alt-ctrl-L` 来重启不是十分理想。在dev模式的Atom窗口下，Atom支持样式的即时更新。

要想开启dev模式的窗口：

通过选择 View > Developer > Open in Dev Mode 菜单，或者按下 `cmd-shift-o` 快捷键来直接在dev模式窗口中打开你的主题。
修改你的主题并保存它。你的修改应该会马上应用。
如果你想要在任何时候都重新加载全部的样式，你可以使用 `cmd-ctrl-shift-r` 快捷键。

## 开发者工具

Atom基于Chrome浏览器，并且支持Chrome开发者工具。你可以选择View > Toggle Developer Tools菜单，或者使用cmd-alt-i快捷键来打开它。

开发者工具允许你查看各个元素，以及他们的CSS属性。

## Atom 样式指南

如果你在创建一个界面主题，你可能想要一种方式来查看你的主题如何影响系统中的组件。样式指南是一个页面，里面渲染了所有Atom支持的组件。

打开命令面板（cmd-shift-P）寻找“styleguide”，或者使用cmd-ctrl-shift-g快捷键来打开样式指南。

![atom-styleguide](http://og8z552x2.bkt.clouddn.com/atom-styleguide.png )

这里列出了所有style，直观多了。通过这里，我知道了如果我想改变边栏选中时的颜色，只要修改background-color-selected 的颜色值就可以了。于是回到 ui-variables.less 中，找到这个变量，改成想要的颜色(#93ffeb)即可。重新加载主题就能看到效果了。

## 变量

### 文本颜色

- @text-color
- @text-color-subtle
- @text-color-highlight
- @text-color-selected
- @text-color-info - 蓝色
- @text-color-success- 绿色
- @text-color-warning - 橙色或者黄色
- @text-color-error - 红色

### 背景颜色

- @background-color-info - 蓝色
- @background-color-success - 绿色
- @background-color-warning - 橙色或者黄色
- @background-color-error - 红色
- @background-color-highlight
- @background-color-selected
- @app-background-color - 所有编辑器组件下面的应用背景

###组件颜色

- @base-background-color -
- @base-border-color -
- @pane-item-background-color -
- @pane-item-border-color -
- @input-background-color -
- @input-border-color -
- @tool-panel-background-color -
- @tool-panel-border-color -
- @inset-panel-background-color -
- @inset-panel-border-color -
- @panel-heading-background-color -
- @panel-heading-border-color -
- @overlay-background-color -
- @overlay-border-color -
- @button-background-color -
- @button-background-color-hover -
- @button-background-color-selected -
- @button-border-color -
- @tab-bar-background-color -
- @tab-bar-border-color -
- @tab-background-color -
- @tab-background-color-active -
- @tab-border-color -
- @tree-view-background-color -
- @tree-view-border-color -
- @ui-site-color-1 -
- @ui-site-color-2 -
- @ui-site-color-3 -
- @ui-site-color-4 -
- @ui-site-color-5 -

### 组件尺寸

- @disclosure-arrow-size -
- @component-padding -
- @component-icon-padding -
- @component-icon-size -
- @component-line-height -
- @component-border-radius -
- @tab-height -

### 字体

- @font-size -
- @font-family -


# 创建语法主题

同时按 `cmd+shift+p`，输入 `generate syntax theme` 并回车，为主题命名，选择一个保存的位置就可以了。注意名字一定要以-syntax结尾。

保存好后，就可以在设置-Themes的Syntax主题列表中看到刚建好的主题了。选择它。Syntax部分比较简单，不需要手动建立链接，目录结构也少了许多。

目录结构

```
codyer-theme-Syntax/
	  └── styles/
	      ├── base.less 
	      ├── colors.less  
	      └── syntax-variables.less	
```
在 `colors.less` 修改自己喜好的配色即可。


## 发布语法主题

- 首先把制造好的主题上传至github。
- 进入主题目录 执行 `apm publish minor` 之后输入github的用户名及密码，会要求输入 atom token，然后上传成功，就可以在 atom编辑器里下载到你自己的主题了。


# 我的自定义主题

主题名称 [codyer-theme-syntax](https://atom.io/themes/codyer-theme-syntax)

![codyer-theme-html](http://og8z552x2.bkt.clouddn.com/atom-js-style.png)


