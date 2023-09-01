---
title: discuz
date: 2021-11-05 13:39:16
tags: discuz
categories: discuz
---


### 运行npm install 出现thon Python is not set from command line or npm configuration解决方案

管理员权限打开执行：npm install --global --production windows-build-tools


### 解决Error: ENOENT: no such file or directory, scandir 安装node-sass报错
npm rebuild node-sass

### npm install 项目时，报：gyp ERR! stack Error: `gyp` failed with exit code: 1

1. > npm install -g cnpm@6.0.0 --registry=https://registry.npm.taobao.org

其中的@6.0.0是指定cnpm的版本号，cnpm版本号一定要和npm版本（npm -v）一致，（如npm6.x.x则cnpm版本为6.x.x，大版本一致即可）

2. > cnpm install

如果`cnpm install`失败，多构建几次尝试

3. > npm install 
会发现`npm install`也可以了

4. 如果`npm run dev`报错，再执行一次`npm install`，可能cnpm有的插件没安装下来？

### gyp ERR! find Python 解决方案
[gyp ERR! find Python 解决方案](https://segmentfault.com/a/1190000023271417)

1.安装node-gyp
```
npm install -g node-gyp
```

2.安装python

### jenkins node.js版本不对解决
1.管理Jenkins - 管理插件 - 下载NodeJS插件

2.下载的插件在：$JENKINS_HOME/plugins目录下

3.管理Jenkins - Global Tool Configuration(全局工具配置) - 选择需要安装的nodejs版本（会从nodejs官网下载安装，nodejs安装包在：$JENKINS_HOME/tools目录下）
![](http://www.guoxh.com/blog/img/Jenkins/1.png)
> 这里可以添加多个node版本，然后就可以实现不同项目使用不同的node版本，以实现项目代码对node版本的要求

2.项目构建环境勾选`Provide Node & npm bin/ folder to PATH`选项
![](http://www.guoxh.com/blog/img/Jenkins/2.png)

> 每次build，都会首先执行环境构建，环境构建无误后，才会开始真正的构建过程
会下载nodejs并安装配置，并把node添加到当前PATH环境变量中，这样就支持node和npm命令啦！


### taro UI按需引用样式文件报错`引用taro UI报错Error: Cannot find module './style/index.scss'`

安装tao-ui时,如果出现报错，可以尝试安装taro-ui版本，因为目前taro ui2.+和taro3.+版本不兼容,使用以下命令可解决

npm install taro-ui@3.0.0-alpha.3

### web端代码不支持修改NODE_ENV环境变量，因为next打包基于production环境变量

### PC发布帖子，小程序不显示

需要再管理后台`全局`-`站点设置`-`功能设置`勾选`打赏、悬赏、红包、匿名、私信、商品、帖子付费、用户组付费、充值`选项

### taro子组件不能直接调用onShow生命周期，所以需要
```js
import Taro, { eventCenter, getCurrentInstance } from '@tarojs/taro';
$instance = getCurrentInstance();
  componentDidMount() {
    const onShowEventId = this.$instance.router.onShow;
    eventCenter.on(onShowEventId, this.onShow);
  }
  onShow = () => {
    //这样就可以调用onShow了
  }
  componentWillUnmount() {
    //始终记得要在这个react组件卸载前生命周期方法里清除onShow事件，否则会一直累加
    const onShowEventId = this.$instance.router.onShow;
    eventCenter.off(onShowEventId, this.onShow);
  }
```