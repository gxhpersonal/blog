---
title: 判断android和ios
date: 2016-09-14 18:07:27
tags:
categories: JS
---

### 判断android和ios
```javascript
var browser = {
versions: function () {
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