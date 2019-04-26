---
title: 关于em rem px
date: 2016-09-14 18:07:27
tags: css
categories: css
---


### js操作rem
```html
<style>html{font-size:100px;}</style>
<script> 
    //屏幕适应 
    (function (win, doc) {
        if (!win.addEventListener) return;
        var html = document.documentElement;
        function setFont() {
            var html = document.documentElement;
            var k = 750;
            html.style.fontSize = html.clientWidth / k * 40 + "px";
        }
        setFont();
        setTimeout(function () {
            setFont();
        }, 300);
        doc.addEventListener('DOMContentLoaded', setFont, false);
        win.addEventListener('resize', setFont, false);
        win.addEventListener('load', setFont, false);
    })(window, document);
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
