---
title: 关于表单
date: 2016-09-14 18:07:27
tags: css
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

### input标签
> IE：不管该行有没有文字，光标高度与font-size一致。

FF：该行有文字时，光标高度与font-size一致。该行无文字时，光标高度与input的height一致。

Chrome：该行无文字时，光标高度与line-height一致；该行有文字时，光标高度从input顶部到文字底部(这两种情况都是在有设定line-height的时候)，如果没有line-height，则是与font-size一致。

解决的方案：

给input的height设定一个较小的高度（或者不设置高度），然后用padding去填充，基本上可以解决所有浏览器的问题

### CSS控制输入文本大小写
> text-transform：none；capitalize；uppercase；lowercase；inherit；
>none默认。定义带有小写字母和大写字母的标准的文本。
capitalize文本中的每个单词以大写字母开头。
uppercase定义仅有大写字母。lowercase定义无大写字母，仅有小写字母。
inherit规定应该从父元素继承 text-transform 属性的值。

### 控制input标签的placeholder属性实现获取焦点显示暗文，失去焦点隐藏(其实就是改变placeholder中的文字颜色)
```css
    &::-webkit-input-placeholder { color:transparent; }
    &:-moz-placeholder { color:transparent; }
    &::-moz-placeholder { color:transparent; }
    &:-ms-input-placeholder { color:transparent;}
    &:focus::-webkit-input-placeholder { color:#9fb0bf; }
    &:focus:-moz-placeholder { color:#9fb0bf; }
    &:focus::-moz-placeholder { color:#9fb0bf; }
    &:focus:-ms-input-placeholder { color:#9fb0bf; }
```

### ios在fixed布局下出现bug问题
软键盘唤起后，页面的fixed 元素将失效（即无法浮动，也可以理解为变成了absolute定位），所以当页面超过一屏且滚动时，失效的 fixed 元素就会跟随滚动了。
这便是 iOS 上 fixed 元素和输入框的 bug 。其中不仅限于 type=text 的输入框，凡是软键盘（比如时间日期选择、select 选择等等）被唤起，都会遇到同样地问题。
解决方法：
    将原 body 滚动的区域域移到 main 内部，而 header 和footer 的样式不变。
.main{
    position: absolute;
    top: 50px;
    bottom: 34px;
    overflow-y: scroll;
}
.main  .content {
    height: 2000px;
}

这样布局可能会是的滚动失去原来的流畅，加以下代码，恢复之前丝滑般的滚动：
    -webkit-overflow-scrolling: touch;

这样的布局在h5是行不通的，所以需要用JS来控制内部滚动元素的高度：
```javascript
// 1. 内部滚动
var w = window.innerWidth,
        h = window.innerHeight;  //获取窗口的高度与宽度(不包含工具条与滚动条):
    $('#js_orderConWrap').height(h - $('.cm-header-wrap').height());  //内部元素的高度 = 窗口高度 - 头部或底部的高度(如果有)

//既然都在h5了，所以还要考虑APP环境，在APP环境下头部是不会用h5的，所以需要判断h5和APP环境；

// 2.弹层显示，禁止背景滚动
//关闭滚动条
        $(document).on('touchmove',function(e){
            if($('.app-popup-container').css('display') === 'block'){
                e.preventDefault();
            }
})
```

### 定义插入光标（caret）的颜色
```css
 caret-color: red;
```

### 修改 chrome 记住密码后自动填充表单的黄色背景
```css
/* chrome表单自动填充后，input文本框的背景会变成黄色的，通过审查元素可以看到这是由于chrome会默认给自动填充的in
put表单加上input:-webkit-autofill私有属性，然后对其赋予以下样式： */
{
background-color: rgb(250,255,189) !important;
background-image: none !important;
color: rgb(0,0,0) !important;
}

/* 对chrome默认定义的background-color，background-image，color使用important是不能提高其优先级的，但是
其他属性可使用。*/

/* 使用足够大的纯色内阴影来覆盖input输入框的黄色背景，处理如下 */
input:-webkit-autofill,textarea:-webkit-autofill,select:-webkit-autofill{
-webkit-box-shadow: 000px 1000px white inset;
border: 1px solid #CCC !important;
}
```