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

