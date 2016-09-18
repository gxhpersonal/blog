---
title: 常用的git命令
date: 2016-09-18 15:48:19
tags:
categories: git
---

### 关于git的几个专用名词
* Workspace：工作区
* Index / Stage：暂存区
* Repository：仓库区（或本地仓库）
* Remote：远程仓库

### 新建代码库
**在当前目录新建一个git代码库**
$ git init

**克隆一个项目**
$ git clone https://github.com/gxhpersonal/blog.git

### 配置
Git的设置文件为`.gitconfig`，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）。

```sh
 **设置提交代码时的用户信息**
git config --global user.name "gxhpersonal"
git config --global user.email "991158744@qq.com"
```