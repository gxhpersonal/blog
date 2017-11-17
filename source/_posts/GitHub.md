---
title: GitHub
date: 2017-11-17 10:15:53
tags: GitHub
categories: GitHub
---

### 如何将本地文件上传到Github上？
#### 1.在目录中创建新的 Git 仓库
进入到你本地项目的根目录下，执行 git init 命令，就可以创建一个 Git 仓库了
![git-init](http://ota5i8p1g.bkt.clouddn.com/git-init.png)

#### 2、执行 git add . 命令，将项目的所有文件提交到暂存区

![git-add-.](http://ota5i8p1g.bkt.clouddn.com/git-add-..png)

#### 3、执行git commit -m "注释语句" 命令，将暂存区内容添加到仓库中

![git-commit](http://ota5i8p1g.bkt.clouddn.com/git-commit.png)

#### 4、在 Github 新建一个 Repository，点击Create repository，跳转到如下页面，获取所创建的仓库的HTTPS地址

![repository](http://ota5i8p1g.bkt.clouddn.com/repository.png)

#### 5、点击copy按钮获取所创建的仓库的HTTPS地址

![copy](http://ota5i8p1g.bkt.clouddn.com/copy.png)

#### 6、将本地的仓库关联到Github上，上传代码到github远程仓库	
```
git remote add origin [GitHub复制的https地址]
```

![git-push](http://ota5i8p1g.bkt.clouddn.com/gitpush.png)

##### bingo！