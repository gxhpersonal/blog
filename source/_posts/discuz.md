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

### 因为微信新增小程序隐私协议的原因，升级taro
1. 全局安装Taro cli:
```
npm install -g @tarojs/cli 
```
2. 查看Taro所有命令及帮助：
```
taro --help
```
3. 更新taro版本：
```
taro updata self
```
4. 更新项目中的Taro相关依赖：
```
taro update project
```
5. 环境及依赖检测
```
taro info
```

更多见官网说明：https://taro-docs.jd.com/docs/cli

但是会有第三方插件不支持最新版taro的情况，所以可以另辟蹊径，采用taro官方补丁插件：

#### @tarojs/plugin-inject
> 可以为小程序平台注入公共的组件、API 等逻辑
##### 版本要求
###### Taro 3.3+
请使用本插件的`1.0`或以上版本
###### Taro 3.1/3.2
请使用本插件的`0.0.2`或以上版本

##### 安装
在 Taro 项目根目录下安装
```
$ npm i @tarojs/plugin-inject --save
```
##### 使用
###### 引入插件
请确保`Taro CLI`已升级至`Taro 3.1.0`的最新版本。

修改项目`config/index.js`中的`plugins`配置为如下:
```js
const config = {
  ...
  plugins: [
    [
      '@tarojs/plugin-inject',
      {
        // 配置需要新增的 API
        syncApis: ['getPrivacySetting', 'openPrivacyContract', 'requirePrivacyAuthorize', 'onNeedPrivacyAuthorization'], //同步API
        asyncApis:[''],//异步
        components: {
          // 配置组件新增 属性和事件
          Button: {
            "open-type": "agreePrivacyAuthorization", //属性写法
            bindAgreePrivacyAuthorization: '', //事件写法（一定要写成小程序官方事件名，不能写成taro事件名）
          },
        },
      },
    ],
  ],
  ...
}
```
> 官方文档：https://github.com/NervJS/taro-plugin-inject

### taro使用原生组件
1. 在`app`或页面配置文件`page.config.js`中配置`usingComponents`属性：
```js
export default {
  usingComponents: {
    // 定义需要引入的第三方组件
    // 1. key 值指定第三方组件名字，以小写开头
    // 2. value 值指定第三方组件 js 文件的相对路径
    'ec-canvas': '../../components/ec-canvas/ec-canvas',
  },
}
```
2. 使用组件：
```js
import React, { Component } from 'react'
import { View } from '@tarojs/components'

export default class Index extends Component {
  this.state = {
    ec: {
      onInit: function () {}
    }
  }

  render () {
    return (
      <View>
        <ec-canvas id='mychart-dom-area' canvas-id='mychart-area' ec={this.state.ec} />
      </View>
    )
  }
}
```

### There are multiple modules with names that only differ in casing报错

一般是组件文件夹名用了驼峰命名，改成`小写`加`-`即可

https://github.com/hsuna/taro-privacy-popup/blob/master/src/components/PrivacyPopup/index.jsx