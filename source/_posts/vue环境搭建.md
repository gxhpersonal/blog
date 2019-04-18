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
```
$ vue init webpack vue-project
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