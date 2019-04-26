---
title: 关于em rem px
date: 2016-09-14 18:07:27
tags: css
categories: css
---


### js操作rem
```html
<script>
(function(f,i){var h=document,d=window,b=h.documentElement,e=document.createElement("style"),c;function g(){var j=b.getBoundingClientRect().width;i=i||540;j>i&&(j=i);var k=j*100/f;e.innerHTML="html{font-size:"+k+"px;}"}if(b.firstElementChild){b.firstElementChild.appendChild(e)}else{var a=h.createElement("div");a.appendChild(e);h.write(a.innerHTML);a=null}g();d.addEventListener("resize",function(){clearTimeout(c);c=setTimeout(g,300)},false);d.addEventListener("pageshow",function(j){if(j.persisted){clearTimeout(c);c=setTimeout(g,300)}},false)})(750,750);
</script>
```

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
