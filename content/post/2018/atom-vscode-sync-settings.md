---
title: Atom和VSCode的插件同步方法
date:  2018-05-12T10:49:46+08:00
tags: ["frontend","Atom","vscode"]
categories: ["frontend"]
---

在我们换电脑和开发环境的时候，自己使用编辑器的一些插件，往往还的重新下载，不过现在这些编辑器提供了，同步方法，下面就简单说一下，不同电脑如何同步编辑器插件。
# Atom 通过sync-settings插件实现同步功能
atom 安装插件
- 安装插件 sync-settings
- 拥有github账户，并设置一个 Personal access tokens
注意，token需要保存好， 用于绑定你的插件

**上述准备好**
- 按下 `Ctrl-Shift-P` 打开命令面板
- 输入 `sync-settings:backup` 将本机配置上传到gist
- 输入 `sync-settings:restore` 将gist上的配置同步到本机，

# VSCode 同步设置及扩展插件 Setting Sync
- 安装插件 Setting Sync
- 同上需要一个github账户，并设置一个 Personal access tokens
## 回到VSCode配置将token配置到本地
`Shift + Alt + U` 在弹窗里输入你的token， 回车后会生成syncSummary.txt文件

Setting Sync 快捷键：
- 上传： `Shift + Alt + U` (Sync: Update / Upload Settings)
- 下载： `Shift + Alt + D` (Sync: Download  Settings)

## 另外一台电脑要同步的时候
(Sync: Download  Settings) `Shift + Alt + D` 在弹窗里输入你的gist值，稍后片刻便可同步成功

## 如果要重置同步设置，变更其它token
Ctrl+P / F1 弹出输入>sync,即可重新配置你的其它token来同步
