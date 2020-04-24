---
title: "用 Hugo 配合 Github Actions 自动构建Blog"
date: 2020-04-21T18:37:16+08:00
tags: ["Hugo", "Github"]
categories: ["frontend"]
---

# 用 Hugo 配合 Github Actions 自动构建Blog

使用hugo来搭建blog，已经有人比较全面的介绍了如何利用github actions来自动构建。

这里我来补充一些细节。

## 初始化 github 仓库

  可以直接创建一个项目，之后创建分支 `develop`、`gh-pages` 分支

## 安装 [hugo](https://gohugo.io/getting-started/)

按照官网步骤很容易

- 1、`brew install hugo`
- 2、`hugo new site quickstart`
- 3、添加主题：`git clone https://github.com/olOwOlo/hugo-theme-even themes/even`

这里 `even` 主题还不错，就安装了。主题内会内置exampleSite文件，进入目录复制，然后在安装目录下覆盖粘贴。

`hugo server -D` 就可以看到内容了。


## Github Actions 自动构建

这里主要说一下自动构建部分，如果没搞过会比较容易摸不着头脑。

首先我们要明白你自动构建监听的是`develop` 分支，步骤如下：

- 检出代码
- 安装 Hugo 环境
- 编译 Hugo
- 将 public 下的文件夹推送到 `gh-pages` 分支

在博客目录下新建 `.github/workflows` 里面新建一个 `gh_pages.yml` 文件，复制以下代码

```
name: GitHub Page Deploy

on:
  push:
    branches:
      - develop
jobs:
  deploy:
    runs-on: ubuntu-18.04
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true  # Fetch Hugo themes
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: '0.69.0'

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          deploy_key: ${{ secrets.ACTIONS_DEPLOY_KEY }}
          publish_dir: ./public
          publish_branch: gh-pages  # deploying branch
```
这里 `hugo-version` 版本根据你实时安装时候的来设置。

以上的配置参数可以参考 [actions-hugo](https://github.com/peaceiris/actions-hugo)、 [github-pages-action](https://github.com/marketplace/actions/github-pages-action)、[actions-gh-pages](https://github.com/peaceiris/actions-gh-pages)这里。


### 创建 SSH Deploy Key

进入目录，使用以下命令生成您的部署密钥。

```
ssh-keygen -t rsa -b 4096 -C "$(git config user.email)" -f gh-pages -N ""
# You will get 2 files:
#   gh-pages.pub (public key)
#   gh-pages     (private key)
```

在目录下会生成 `gh-pages.pub` 和 `gh-pages` 两个文件。

然后在 github 上面你的项目中找到 `Settings` 标签，点击进入，在左边 `options` 列表里：
- 找到 `Deploy keys` 点击 `Add deploy keys` 将上面生成文件 `gh-pages.pub` 的内容粘贴进去并保存。
- 找到 `Secrets` 点击 `Add a new secrets` 将上面生成文件 `gh-pages` 的内容粘贴进去并保存。

这样我们的 SSH Deploy Key 就创建完成。

## 推送到 Github

复制如下代码新建文件 `deploy.sh`

```
#!/bin/bash

echo -e "\033[0;32mDeploying updates to GitHub...\033[0m"

# Build the project.
hugo -t even

# Add changes to git.
git add .

# Commit changes.
msg="rebuilding site `date`"
if [ $# -eq 1 ]
  then msg="$1"
fi
git commit -m "$msg"

# Push source and build repos.
git push origin develop
```
运行 `bash deploy.sh` 执行上面的代码后，Github 收到 PUSH 后 Actions 就会自动开始构建了。


> 原文章： [用 Hugo 配合 Github Actions 自动构建我的博客](https://www.nashome.cn/post/hugo-github-actions/)