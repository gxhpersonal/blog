---
title: 前端与原生交互
date: 2018-01-22 14:25:23
tags: Native
categories: Native
---

### iOS中时间格式
ios不支持时间格式如：new Date(2018-01-24)格式转换,会抛出NaN,所以要转换成`/`格式的，如：
```
let date = 2018-02-12;
let newDate = date.replace(/-/g,'/');
let returnDate = new Date(newDate)
```

### iOS11.XX以上的iPhone注意
1.如果页面中有弹窗有input标签的，此弹窗（包括所有input的父级元素）不能用fixed布局，当弹窗被键盘顶上去，会导致input光标错乱，而且弹窗上面的按钮无法点击；
解决办法：弹层改为position：absolute，其他所有元素都隐藏，相当于页面只留一个弹层，不再让弹层脱离流

### iOS环境css :active伪元素不起作用
```
document.body.addEventListener('touchstart', function () { })
```
如上，要给body或者点击的元素加touchstart事件，来触发:active伪元素

### IOS支持3D touch的手机如果页面有a标签长按会触发3D touch并且跳转浏览器