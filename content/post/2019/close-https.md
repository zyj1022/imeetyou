---
title: 如何关闭常见浏览器的 https 功能
date:  2019-05-08T11:06:16+08:00
tags: ["frontend", "https"]
categories: ["frontend"]
---


在日常开发的过程中，有时我们会想测试页面在 https 连接中的表现情况，这时 https 的存在会让调试不能方便的进行下去。

而且由于 https 并不是像 cookie 一样存放在浏览器缓存里，简单的清空浏览器缓存操作并没有什么效果，页面依然通过 https 的方式传输。

那么怎样才能关闭浏览器的 https 呢，在这里汇总一下几大常见浏览器 https 的关闭方法。

## Safari 浏览器
- 完全关闭 Safari
- 删除 `~/Library/Cookies/HSTS.plist` 这个文件
- 重新打开 Safari 即可
- 极少数情况下，需要重启系统

## Chrome 浏览器
- 地址栏中输入 `chrome://net-internals/#hsts`
- 在 Delete domain 中输入项目的域名，并Delete 删除
- 可以在 `Query domain` 测试是否删除成功
- Opera 浏览器
- 和 Chrome 方法一样

## Firefox 浏览器
- 关闭所有已打开的页面
- 清空历史记录和缓存
- 地址栏输入 `about:permissions`
- 搜索项目域名，并点击 `Forget About This Site`
