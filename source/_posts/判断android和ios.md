---
title: 判断android和ios
date: 2016-09-14 18:07:27
tags: JS
categories: JS
---

### 与APP交互
很多时候APP中会嵌套H5的页面，这个时候与APP的交互就显得尤为重要，一般我们与APP交互会在window对象下构造一个共用函数，并且需要时返回指定约定的值，APP那边会根据约定的值去执行相应的操作，从而达到交互的目的；
```javascript
//如：window.nativeBack = function(){return 1}
```

### 判断android和ios
```javascript
var browser = {
versions: function () {
//注：有的浏览器是可以改变userAgent信息的
var u = navigator.userAgent, app = navigator.appVersion;
return { //移动终端浏览器版本信息 
ios: !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/), //ios终端 
android: u.indexOf('Android') > -1 || u.indexOf('Linux') > -1, //android终端或uc浏览器 
iPhone: u.indexOf('iPhone') > -1, //是否为iPhone或者QQHD浏览器 
iPad: u.indexOf('iPad') > -1, //是否iPad 
};
}(),
}
if (browser.versions.iPhone || browser.versions.iPad || browser.versions.ios) {
//说明是ios系统
window.location.href="";
}
if (browser.versions.android) {
//说明是Android系统
window.location.href="";
}
```