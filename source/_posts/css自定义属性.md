---
title: css自定义属性
date: 2019-05-07 09:36:27
tags: css
categories: css
---

### css自定义属性

> CSS自定义属性 对于很多前端同学可能并没有接触过，甚至没有听说过，但如果说CSS的变量，估计就很多同学有听说过。但这里再次强调，我们应该纠正这样的说法：
CSS没有变量，没有变量，没有变量；只有自定义属性，只有自定义属性，只有自定义属性！
```html
<!-- HTML -->
<div>我是什么颜色？</div>
<p>那我是什么颜色？</p>
<div class="alert">我又是啥颜色？<p>那我呢？</p></div>
<!-- CSS -->
<style>
:root{
    --color:red;
}
div{
    --color:yellow;
}
.alert{
    --color:blue;
}
*{
    color:var(--color);
}
</style>
```
* :root表示是在根元素上声明的，--color: red，将会影响所有的元素
* div是一个标签元素选择器，其声明的--color: yellow将会影响所有div元素以及他的后代元素
* .alert是一个类选择器，其声明的--color: blue会影响类名为.alert的元素以及其后代码元素所以在第一个div的文本颜色是一个yellow(它覆盖了:root的red颜色)；第一p的文本颜色是red（运用了:root的red色），第二个div.alert以及它的子元素p的文本颜色是blue（运用了.alert中的blue）。