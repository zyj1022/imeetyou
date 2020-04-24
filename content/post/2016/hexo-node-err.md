---
title: 重装node导致Hexo不能正常使用解决办法
date: 2016-11-22 10:40:15
categories:
- frontend
tags:
- hexo
- frontend
---

Hexo 是一个简单地、轻量地、基于Node的一个静态博客框架。最近重装了node，导致在编译博客的时候，会出现很多依赖的错误。比如：

在使用hexo过程中，使用node 6.0以上版本，会出现fs版本问题。

# 问题一

```
ATAL Error: Module version mismatch. Expected 48, got 14.
Template render error: Error: Module version mismatch. Expected 48, got 14.
```

错误提示模块的版本不匹配，可能是因为重装了node，很多模块更新或者确实，所以我们重新安装配置下Hexo,解决方法：执行以下代码

```
npm install hexo --no-optional
```

<!-- more -->

# 问题二

安装Hexo时，执行“npm install -g hexo-cli“出现错误

```
npm ERR! tar.unpack untar error /Users/Macx/.npm/hexo-cli/0.1.8/package.tgz
npm ERR! Darwin 14.4.0
npm ERR! argv "/usr/local/bin/node" "/usr/local/bin/npm" "install" "hexo-cli" "-g"
npm ERR! node v4.2.1
npm ERR! npm v2.14.7
npm ERR! path /usr/local/lib/node_modules/hexo-cli
npm ERR! code EACCES
npm ERR! errno -13
npm ERR! syscall mkdir

npm ERR! Error: EACCES: permission denied, mkdir '/usr/local/lib/node_modules/hexo-cli'
npm ERR! at Error (native)
npm ERR! { [Error: EACCES: permission denied, mkdir '/usr/local/lib/node_modules/hexo-cli']
npm ERR! errno: -13,
npm ERR! code: 'EACCES',
npm ERR! syscall: 'mkdir',
npm ERR! path: '/usr/local/lib/node_modules/hexo-cli',
npm ERR! fstream_type: 'Directory',
npm ERR! fstream_path: '/usr/local/lib/node_modules/hexo-cli',
npm ERR! fstream_class: 'DirWriter',
npm ERR! fstream_stack: 
npm ERR! [ '/usr/local/lib/node_modules/npm/node_modules/fstream/lib/dir-writer.js:35:25',
npm ERR! '/usr/local/lib/node_modules/npm/node_modules/mkdirp/index.js:47:53',
npm ERR! 'FSReqWrap.oncomplete (fs.js:82:15)' ] }
npm ERR! 
npm ERR! Please try running this command again as root/Administrator.

npm ERR! Please include the following file with any support request:
npm ERR! /Users/Macx/Desktop/GitHub/npm-debug.log
```


分析下错误，异常可能因为权限问题，所以我们执行一些安装命令是需要申请root执行权限。

解决方法：执行以下代码:


```
sudo npm install --unsafe-perm --verbose -g hexo
```
