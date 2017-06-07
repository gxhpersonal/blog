---
title: jquery常见问题
date: 2016-12-29 10:04:56
tags:
categories: jquery
---
###trigger() 方法
给指定元素添加触发事件，例：$("#this").trigger("click")

###jQuery noConflict() 方法
noConflict() 方法会释放会 $ 标识符的控制，这样其他脚本就可以使用它了。

```script
var jq = $.noConflict();
jq(document).ready(function(){
  jq("button").click(function(){
    jq("p").text("jQuery 仍在运行！");
  });
});
```

### ready()方法
当 DOM（文档对象模型） 已经加载，并且页面（包括图像）已经完全呈现时，会发生 ready 事件。
>ready() 函数仅能用于当前文档，因此无需选择器。
语法 1
$(document).ready(function)
语法 2
$().ready(function)
语法 3
$(function)
* 提示：ready() 函数不应与 <body onload=""> 一起使用。