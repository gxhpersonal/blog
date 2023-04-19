---
title: 常用的git命令
date: 2016-09-18 15:48:19
tags: git
categories: git
---

### 关于git的几个专用名词
* Workspace：工作区
* Index / Stage：暂存区
* Repository：仓库区（或本地仓库）
* Remote：远程仓库

### 新建代码库

	//在当前目录新建一个git代码库
	$ git init

	//克隆一个项目
	$ git clone https://github.com/gxhpersonal/blog.git

### 配置
Git的设置文件为`.gitconfig`，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）。

	// 设置提交代码时的用户信息
	git config --global user.name "gxhpersonal"
	git config --global user.email "991158744@qq.com"

### 查看信息
	//显示有变更的文件
	$ git status


### 增加/删除文件

	//添加当前目录的所有文件到暂存区
	$ git add .
	
	//添加每个变化前，都会要求确认
	//对于同一个文件的多处变化，可以实现分次提交
	$ git add -p

	//删除工作区文件，并且将这次删除放入暂存区
	$ git rm [file1] [file2] ...

	//改名文件，并且将这个改名放入暂存区
	$ git mv [file-original] [file-renamed]

### 代码提交

	//提交暂存区到仓库区
    $ git commit -m "改的内容标题"

	//提交工作区自上次commit之后的变化，直接到仓库区
	$ git commit -a

### 新建的分支push到远程服务器上
    $ git push -u origin [分支名]
### 远程代码取到本地

	$ git pull

### 本地代码提交到远程仓库

    $ git push

### 分支

```git
	 //列出所有本地分支
	$ git branch

	//列出所有远程分支
	$ git branch -r

	//列出所有本地分支和远程分支
	$ git branch -a

	//新建一个分支，但依然停留在当前分支
	$ git branch [branch-name]

	//新建一个分支，并切换到该分支
	$ git checkout -b [branch]

	//切换到指定分支，并更新工作区
	$ git checkout [branch-name]

	//切换到上一个分支
	$ git checkout -

	//合并指定分支到当前分支
	$ git merge [branch]

	//选择一个commit，合并进当前分支
	$ git cherry-pick [commit]

	//删除本地分支
	$ git branch -d [branch-name]

	//删除远程分支
	$ git push origin :[branch-name]
```

### 指定某个commit到指定的分支
1.执行git log -3 --graph test，查看test分支下的commit:
 
注：commit 后面的hash值代表某个commit，这里把”2e1ada53819d46557b24ee7376dc61d37a06939d“这个commit提交到master。
2.执行git checkout master，切换到master分支。

3.执行 git cherry-pick 2e1ada53819d46557b24ee7376dc61d37a06939d，该commit便被提交到了master分支。
 
到此，”2e1ada53819d46557b24ee7376dc61d37a06939d“这个commit便被提交到了master分支。

### 回退到某个commit，并提交远程
回退命令：
```git
$ git reset --hard HEAD^         回退到上个版本
$ git reset --hard HEAD~3        回退到前3次提交之前，以此类推，回退到n次提交之前
$ git reset --hard commit_id     退到/进到 指定commit的sha码
```

强推到远程：
```git
$ git push origin HEAD --force
```

> 如果强推的分支为受保护分支（受保护分支是不允许进行 【--force】操作），需要修改项目设置中受保护分支选项：
> 找到项目远程地址Settings ——> Repository ——> Protected Branches ——> Unprotect 撤销保护分支

### git提交GitHub代码不再需要每次commit输入username和password解决
1.在命令行输入命令:
```
git config --global credential.helper store
```
☞ 这一步会在用户目录下的.gitconfig文件最后添加:
```
 [credential]
     helper = store
```
2.现在push你的代码 (git push), 这时会让你输入用户名密码, 这一步输入的用户名密码会被记住, 下次再push代码时就不用输入用户名密码啦!
☞这一步会在用户目录下生成文件.git-credential 用来记录用户名密码的信息.
☞ git config --global 命令实际上在操作用户目录下的.gitconfig文件, 我们cat一下此文件(cat .gitconfig), 其内容如下:
```
[user]
 name = alice
 email = alice@aol.com
[push]
 default = simple
[credential]
 helper = store
```
* 拓展：
```
git config --global user.email "alice@aol.com" 操作的就是上面的email
git config --global push.default matching 操作的就是上面的push段中的default字段
git config --global credential.helper store 操作的就是上面最后一行的值
```

### 更多


### git bisect 命令
它的原理很简单，就是将代码提交的历史，按照两分法不断缩小定位。所谓"两分法"，就是将代码历史一分为二，确定问题出在前半部分，还是后半部分，不断执行这个过程，直到范围缩小到某一次代码提交。
[阮一峰老师博客](http://www.ruanyifeng.com/blog/2018/12/git-bisect.html)