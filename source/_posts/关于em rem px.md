---
title: 关于定位
date: 2016-09-14 18:07:27
tags:
categories: css
---

### px em rem
> 1.em
一般都是 body 的 font-size 为基准，例如：
```css
body {
    font-size: 62.5%;
    /*10 ÷ 16 × 100% = 62.5%*/
}
h1 {
    font-size: 2.4em;
    /*2.4em × 10 = 24px */
}
```
计算公式：1 ÷ 父元素的font-size × 需要转换的像素值 = em值

2.rem
```css
html {
    font-size: 62.5%;
    /*10 ÷ 16 × 100% = 62.5%*/
}
body {
    font-size: 1.4rem;
    /*1.4 × 10px = 14px */
}
h1 {
    font-size: 2.4rem;
    /*2.4 × 10px = 24px*/
}
```
