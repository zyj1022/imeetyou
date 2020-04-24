---
title: 关于npm build错误的奇特问题
date: 2019-04-16T13:27:36+08:00
tags: ["frontend", "npm"]
categories: ["frontend"]
---

`npm run build` 后发生的问题：

```
0 info it worked if it ends with ok
1 verbose cli [ '/usr/local/bin/node', '/usr/local/bin/npm', 'start' ]
2 info using npm@6.9.0
3 info using node@v8.11.1
4 verbose run-script [ 'prestart', 'start', 'poststart' ]
5 info lifecycle ui-new@1.0.0~prestart: ui-new@1.0.0
6 info lifecycle ui-new@1.0.0~start: ui-new@1.0.0
7 verbose lifecycle ui-new@1.0.0~start: unsafe-perm in lifecycle true
8 verbose lifecycle ui-new@1.0.0~start: PATH: /usr/local/lib/node_modules/npm/node_modules/npm-lifecycle/node-gyp-bin:/Users/zhi/JD/ui-new/node_modules/.bin:/Library/Frameworks/Python.framework/Versions/3.5/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Applications/VMware Fusion.app/Contents/Public
9 verbose lifecycle ui-new@1.0.0~start: CWD: /Users/zhi/JD/ui-new
10 silly lifecycle ui-new@1.0.0~start: Args: [ '-c', 'npm run dev' ]
11 silly lifecycle ui-new@1.0.0~start: Returned: code: 130  signal: null
12 info lifecycle ui-new@1.0.0~start: Failed to exec start script
13 verbose stack Error: ui-new@1.0.0 start: `npm run dev`
13 verbose stack Exit status 130
13 verbose stack     at EventEmitter.<anonymous> (/usr/local/lib/node_modules/npm/node_modules/npm-lifecycle/index.js:285:16)
……
……
……
```
## 解决方案
- npm 降级

安装并设定一个符合的python版本
```
npm install --python=python2.7
npm config set python python2.7
```
