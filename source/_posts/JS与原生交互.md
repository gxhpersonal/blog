---
title: JS与原生交互
date: 2018-01-22 14:25:23
tags: Native
categories: Native
---

### IOS中时间格式
ios不支持时间格式如：new Date(2018-01-24)格式转换,会抛出NaN,所以要转换成`/`格式的，如：
```
let date = 2018-02-12;
let newDate = date.replace(/-/g,'/');
let returnDate = new Date(newDate)
```
