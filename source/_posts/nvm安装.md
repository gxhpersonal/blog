---
title: nvm安装
date: 2023-09-06 14:35:42
categories:
tags: nvm
---

### 之前一直懒没有安装nvm，现在安装上发现又简单又方便，真香！

### 注意：在下载nvm之前需要卸载本电脑已经安装的node!

### 一、进入官网`http://nvm.uihtm.com/` 或者github`https://github.com/coreybutler/nvm-windows/releases` 下载：

![](http://www.guoxh.com/blog/img/nvm/1.png)

### 二、安装：
一路向下就好了，没什么好说的：
![](http://www.guoxh.com/blog/img/nvm/2.png)
![](http://www.guoxh.com/blog/img/nvm/3.png)

### 三、打开默认安装的路径：

图中nodejs表示当前激活的node版本：
![](http://www.guoxh.com/blog/img/nvm/4.png)

图中nvm文件夹下可以看到你安装的所有node版本文件（如何安装node后面会说）：
![](http://www.guoxh.com/blog/img/nvm/5.png)

### 四、打开cmd终端
1. 输入`nvm`查看nvm版本及其相关命令信息：
![](http://www.guoxh.com/blog/img/nvm/6.png)

2. 快速查询部分node版本（当然只是部分，其他也可以下载）：
![](http://www.guoxh.com/blog/img/nvm/7.png)

3. 安装指定node版本（下载的node会安装到上面提到的nvm文件夹下）：
```
nvm install [node版本号]
```
![](http://www.guoxh.com/blog/img/nvm/8.png)
<center><b><font size ='1'>如图所示，同时会下载对应npm版本</font></b></center>

4. 切换指定node版本：
```
nvm use 20.x.x
```
![](http://www.guoxh.com/blog/img/nvm/9.png)

5. 特别注意！！！执行`nvm use 20.x.x`命令后，看到下面这样才算切换版本成功：
![](http://www.guoxh.com/blog/img/nvm/10.png)
如果切换后没有显示`(Currently using 64-bit executable)`，则切换失败，执行`node -v`会提示找不到node
可以尝试切换管理员运行模式打开cmd
* win11:导航栏中间图标右键`终端（管理员）`打开cmd
* win10:文件夹右键以管理员模式运行
* 如果还不行，卸载nvm重装