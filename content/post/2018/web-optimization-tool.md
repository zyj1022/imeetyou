---
title: 前端利器，6款开源的Web性能优化辅助工具推荐
date:  2018-02-26T13:07:21+08:00
tags: ["frontend","web"]
categories: ["frontend"]
---

Web 性能优化是一个老生常谈的话题，也是前端页面开发十分重要的部分。当页面加载速度越慢，用户流失的概率就越大，性能和交互直接影响用户体验。

下面推荐几款 Web 性能优化辅助工具推荐，希望能对大家有所帮助。

## Lighthouse
Lighthouse 是 Google 开源的一个自动化工具，用于改进网络应用的质量。你可以将其作为一个 Chrome 扩展程序运行，或从命令行运行。

当为 Lighthouse 提供一个要审查的网址，它将针对此页面运行一连串的测试，然后生成一个有关页面性能的报告。可以参考失败的测试，看看可以采取哪些措施来改进应用。

Chrom 扩展则会把报告以非常人性化的图形界面展示给你。

传送门：[www.oschina.net/p/lighthouse](www.oschina.net/p/lighthouse)

##  Speed Racer
SpeedRacer 是一款性能测试工具，它在 Chrome 中运行脚本，并生成详细的性能报告。

SpeedRacer 是直接借助浏览器来实际测试性能的工具，在实际工作中，可以与其它模拟用户访问流量来评估性能的工具配合使用。

传送门：[https://github.com/speedracer/speedracer](https://github.com/speedracer/speedracer)

## Yellow Lab Tools
Yellow Lab Tools 是一款 Web 性能及前端质量测试工具。与其他工具不同的是，它有一些在其他工具上无法看到的独特功能，例如页面加载时 JavaScript 与 DOM 互动和其他程序代码验证问题。

Yellow Lab Tools 偏向于一个发现不良实践的工具，会综合页面权重、请求数、DOM、错误的 Javascript、错误的 CSS 等方面取得一个评分。并显示出在加载页面的过程中，DOM 是如何相互影响。
传送门：[https://yellowlab.tools/](https://yellowlab.tools/)

## Web Tracing Framework
Web Tracing Framework 也是 Google 推出的一组用于跟踪和调查复杂 Web 应用的库、工具和可视化工具合集。它可以帮助发现性能问题，跟踪回归，并构建流畅的 60fps Web 应用。能让你花更少时间来测试代码即可。


传送门：[www.oschina.net/p/tracing-framework](www.oschina.net/p/tracing-framework)

## grunt-perfbudget
grunt-perfbudget 是一款用于评估性能的 Grunt task，它使用 WebPagetest 的公有或私有实例在特定的 URL 进行测试，并将测试结果和你预期的性能期望做比较。

如果小于预期，那么这个 task 就顺利完成了，如果超过了预期的性能期望，那么就会报告失败，并帮助你分析超出预期的原因。


传送门：[https://github.com/tkadlec/grunt-perfbudget](https://github.com/tkadlec/grunt-perfbudget)

## Sitespeed.io
Sitespeed.io 是一组基于最佳实践以及一些加载时序等量化标准的开源工具，用以帮助开发者分析网页的加载速度和渲染性能。

Sitespeed.io 从开发者的站点收集多个页面的数据，并根据最佳实践等规则来分析这些网页，然后将结果以 HTML 的形式输出，或者以数值的形式发送到 Graphite 。

传送门：[https://www-origin.sitespeed.io/](https://www-origin.sitespeed.io/)

