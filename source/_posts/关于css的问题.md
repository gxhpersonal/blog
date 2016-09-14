---
title: 关于css的问题
date: 2016-09-14 18:07:27
tags:
categories: css
---

### 表单
> 关于input表单的一些样式和JS问题：
input的placeholder属性字体颜色：
```css
        input::-webkit-input-placeholder {
            color: #aaa;
        }
        input:-moz-placeholder {
            color: #aaa;
        }
        input::-moz-placeholder {
            color: #aaa;
        }
        input:-ms-input-placeholder {
            color: #aaa;
        }
```
input的选中状态：
```css
       input:focus{ border:1px solid red }
```

### 关于弹层问题
> 如果有两个模块，这两个模块同时需要浮动，并且没法嵌套；

### 居中
> 绝对定位：
```css
position:relative;
top:50%;
left:50%;
transform:translate(-50%,-50%);
```
或者：
```css
position:relative;
left:50%;
margin-left:自身宽度/2;
```

相对定位：
```css
position:absolute;
top:0;
left:0;
bottom:0;
right:0;
margin:auto;
```

想要在 h1 或 p 标签中加个 i 标签(作用是加个背景图)并且其中的文字垂直居中对齐，可以在其中插入一个 span 标签
父元素设置 height, span 标签设置 display:inline-block; vertical-align:top;

### 清除所有a标签在点击时出现的特效
> 清除所有a标签在点击时出现的特效：a{ -webkit-tap-highlight-color:rgba(255,0,0,0);}

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

### 文字与图片不能垂直居中对齐
> 给图片添加css样式
 vertical-align:text-bottom；

 vertical-align只适用于行内块元素

### img标签和同级div之间的间隙处理
> 给img标签设置 vertical-align:top;display:block;
