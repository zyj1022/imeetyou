---
title: mac格式化U盘方法（解决win下只有200MB的问题）
date: 2016-11-07 9:44:34
categories:
- frontend
tags:
- mac
- frontend
---

有些朋友会发现在Mac上格式化的U盘放到windows的电脑上就只剩下200MB了，这是因为你在格式化时选择了guid分区，而win上只能识别一个分区，所以就只显示了200MB的那一个，接下来，我具体说一下方法~


- 将U盘插入Mac电脑，然后打开磁盘工具
- 注意这一步，1号栏选exfat，因为他传输文件没有4GB的限制。2栏选主引导分布选项，这样的话就不会给你分两个区了，然后点抹掉，意思就是格式化。

<!-- more -->

![mac-u](http://og8z552x2.bkt.clouddn.com/mac-u.jpg)

- 再插到win电脑上看一下，是不是成功了，就这么简单！
