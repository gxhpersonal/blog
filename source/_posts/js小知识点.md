---
title: 知识点
date: 2016-09-14 18:07:27
tags:
categories: JS
---

### 锚点
> scrollIntoView方法：
var dd = document.getElementById("nn").scrollIntoView(true);

### 内部滚动
> window.innerHeight:
var h = window.innerHeight,w=window.innerWidth;
$(document).height(h);
//解决ios不兼容position:fixed 和 适应不同手机高度；