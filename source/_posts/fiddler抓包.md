---
title: fiddler抓包
date: 2019-04-25 14:12:05
tags: fiddler
categories: fiddler
---

[fiddler官方文档](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/ConfigureForiOS)

### 1.IOS使用Fiddler抓包配置
1.点击 `Tools > Options` 确保选中`允许远程客户端连接`,端口默认8888：
![](http://www.guoxh.com/blog/img/fiddler/fiddler-1.png)

2.将鼠标悬停在Fiddler工具栏最右侧的在线指示器`Online`上，以显示分配给Fiddler机器的IP地址:
![](http://www.guoxh.com/blog/img/fiddler/fiddler-2.png)

3.浏览器(必须safari)中输入`ip:8888`,点击下方安装证书按钮，下载证书：
![](http://www.guoxh.com/blog/img/fiddler/fiddler-3.png)

4.在设置中会显示一行下载的证书按钮

5.在iOS 10及更高版本上，安装FiddlerRoot证书后，转到`设置 - > 常规 - > 关于 - > 证书信任设置`，并手动启用FiddlerRoot根证书的完全信任。接受对话框，说明这将允许第三方窃听您的所有通信。

### 2.卸载FiddlerRoot证书
如果您决定卸载根证书：

点按“ 设置”应用。

点击常规。

滚动到个人资料。

点击DO_NOT_TRUST_FiddlerRoot *个人资料。

点击删除。