---
title: css关于Grid布局
date: 2023-04-21 15:53:27
tags: css
categories: css
---

### 如果说flex布局是一维布局，则grid布局则是二维布局

<style>
.wrap{
    display:grid;
    grid-template-columns:repeat(3,100px);
    grid-template-rows:repeat(4,100px);
    grid-template-areas: 'a b c' 'd e f' 'g h i';
    grid-gap:10px 20px;
}
.wrap div{
    display:grid;
    align-items:center;
    justify-items:center;
    place-items:center center; /* place-items是上面两个属性的简写形式 */
    font-size:20px;
    color:#fff;
}
.a1{
    background:blueviolet;
    grid-area:a;
    grid-column:1/3;
    grid-row:1/3;
}
.a2{
    background:lightblue;
}
.a3{
    background:cadetblue;
}
.a4{
    background:cornflowerblue;
}
.a5{
    background:darkblue;
}
.a6{
    background:deepskyblue;
}
.a7{
    background:dodgerblue;
}
.a8{
    background:lightsteelblue;
}
.a9{
    background:powderblue;
    /* grid-area:i;
    grid-column:1/2;
    grid-row:1/3; */
}
</style>
<div class="wrap">
    <div class="a1">1</div>
    <div class="a2">2</div>
    <div class="a3">3</div>
    <div class="a4">4</div>
    <div class="a5">5</div>
    <div class="a6">6</div>
    <div class="a7">7</div>
    <div class="a8">8</div>
    <div class="a9">9</div>
</div>

以上布局充分体现出了`grid`布局的精髓，`grid`布局虽然属性之多，但是随之提供的是更灵活，更多样的布局方式；
```css
/* 多行布局使用repeat，如果不固定元素数量，则可以使用`auto-fill`参数 */
.wrap{
    grid-template-rows:repeat(auto-fill,100px);
}
```