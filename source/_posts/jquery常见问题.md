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