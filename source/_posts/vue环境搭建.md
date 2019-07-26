---
title: vue环境搭建
date: 2019-03-25 11:46:27
tags: vue
categories: vue
---

### 1.全局安装 vue-cli
```
$ npm install --global vue-cli
```

### 2.创建一个基于 webpack 模板的新项目
```cmd
cli2.X快速构建项目：
$ vue init webpack vue-project

cli3.0快速构建项目：
vue create vue-project；
```

### 3.安装依赖
```
//定位vue项目文件夹下
$ cd vue-project
//安装依赖
$ npm install
```

### 4.安装scss(SCSS 是 Sass 3 引入新的语法,并且继承了 Sass 的强大功能)
```
//直接安装sass可以使用scss
npm install --save-dev sass-loader
//sass-loader依赖于node-sass
npm install --save-dev node-sass
```
```style
<style lang="scss" scope>
</style>
```

### vue打包静态资源js，css路径不对的解决办法
打开config/index.js，将其中的build配置下的assetsPublicPath值改为’./’

### 不同环境配置不同api域名
1.安装cross-env
```npm
npm i --save-dev cross-env
```

2.修改各环境下的参数
在config/目录下添加test.env.js
修改prod.env.js里的内容，修改后的内容如下：
```js
'use strict'
module.exports = {
 NODE_ENV: '"production"',
 EVN_CONFIG:'"prod"',
 API_ROOT:'"https://这里是正式域名.com"'
}
```

3.修改test.env.js文件内容，如下：
```js
'use strict'
module.exports = {
 NODE_ENV: '"testing"',
 EVN_CONFIG:'"test"',
 API_ROOT:'"https://这里是测试域名test.com/"'
}
```

4.对dev.env.js文件内容进行修改，修改后的内容如下:
```js
'use strict'
const merge = require('webpack-merge')
const prodEnv = require('./prod.env')

module.exports = merge(prodEnv, {
  NODE_ENV: '"development"',
  VN_CONFIG: '"dev"',
  API_ROOT: '"https://ticket-api.jia-expo.com/"'
 })
```
> dev环境配制了服务代理，API_ROOT是配制的代理地址

5.修改项目package.json文件
```json
"scripts": {
    "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
    "start": "npm run dev",
    "build": "node build/build.js",
    "build:test": "cross-env NODE_ENV=production env_config=test node build/build.js",
    "build:prod": "cross-env NODE_ENV=production env_config=prod node build/build.js"
  },
```

6.修改config/index.js
```js
build: {
    // 添加test prod 环境的配制
    prodEnv: require('./prod.env'),
    testEnv: require('./test.env'),
```

7.在webpackage.prod.conf.js中使用构建环境参数
```js
// 个性env常量的定义
const env = config.build[process.env.env_config + 'Env']
```

8.调整build/build.js　
```js
'use strict'
require('./check-versions')()
// process.env.NODE_ENV = 'production'  // 注释掉的代码
const ora = require('ora')
const rm = require('rimraf')
const path = require('path')
const chalk = require('chalk')
const webpack = require('webpack')
const config = require('../config')
const webpackConfig = require('./webpack.prod.conf')
// 修改spinner的定义
// const spinner = ora('building for production...')
var spinner = ora('building for ' + process.env.NODE_ENV + ' of ' + process.env.env_config+ ' mode...' )
spinner.start()
//其它内容不需要做任何调整 ...
```

9.大功告成
request中就可以区分接口域名了变量：process.env.API_ROOT
执行`npm run build:test`打包的就是测试环境
执行`npm run build:prod`打包的就是生产环境


## 下面是我遇到的一些问题：
### 路由找不到 || 文件路径找不到报错解决
```js
//新建一个notFound.vue文件来指向*所有路径
const router = new VueRouter({
  mode: 'history',
  routes: [
    { path: '*', component: NotFoundComponent }
  ]
})
```

### background设置背景图片，打包后图片加载不出来
首先可以肯定为路径问题
方法：
1.extract-text-webpack-plugin插件(作用：将捆绑包或捆绑包中的文本提取到单独的文件中)：
> 在build/utils.js文件下找到ExtractTextPlugin.extract参数
> 添加配置项：publicPath: '../../';
> 用的3.0.0版本；

2.用了vue-cli构建项目，会在项目下有个static文件夹，把图片放到这个静态文件夹下，然后使用`/static/logo.png`相对路径引用，可以在本地和打包环境正常显示；