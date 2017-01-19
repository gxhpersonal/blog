---
title: 其他css问题
date: 2016-09-14 18:07:27
tags:
categories: css
---

### flex布局

### 清除所有a标签在点击时出现的特效
> 清除所有a标签在点击时出现的特效：a{ -webkit-tap-highlight-color:rgba(255,0,0,0);}

### 文字与图片不能垂直居中对齐
> 给图片添加css样式
vertical-align:text-bottom；
vertical-align只适用于行内块元素

### img标签和同级div之间的间隙处理
> 给img标签设置 vertical-align:top;display:block;

### 文本超出后显示省略号
> p{
   width:40px;
   white-space: nowrap;
   text-overflow: ellipsis;
   overflow: hidden;
}
p{
    overflow: hidden;                //超出部分隐藏
    text-overflow: ellipsis;         //显示省略号来修饰被剪掉的文本
    display: box;
    display: -webkit-box;
    line-clamp: 3;                     //非规范属性，实现规定文本行数      火狐不支持这个属性
    -webkit-line-clamp: 3;
    -webkit-box-orient: vertical;    //规定框的子元素应该被水平或垂直排列
    -moz-box-orient: vertical;        //支持火狐的写法
} 

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
1. 内部滚动
var w = window.innerWidth,
        h = window.innerHeight;  //获取窗口的高度与宽度(不包含工具条与滚动条):
    $('#js_orderConWrap').height(h - $('.cm-header-wrap').height());  //内部元素的高度 = 窗口高度 - 头部或底部的高度(如果有)

//既然都在h5了，所以还要考虑APP环境，在APP环境下头部是不会用h5的，所以需要判断h5和APP环境；

2.弹层显示，禁止背景滚动
//关闭滚动条
        $(document).on('touchmove',function(e){
            if($('.app-popup-container').css('display') === 'block'){
                e.preventDefault();
            }
})
```
### 自定义光标
> cursor: url(/路径/cursor.cur),auto;        后缀似乎必须为cur

### input标签
> IE：不管该行有没有文字，光标高度与font-size一致。

FF：该行有文字时，光标高度与font-size一致。该行无文字时，光标高度与input的height一致。

Chrome：该行无文字时，光标高度与line-height一致；该行有文字时，光标高度从input顶部到文字底部(这两种情况都是在有设定line-height的时候)，如果没有line-height，则是与font-size一致。

解决的方案：

给input的height设定一个较小的高度，然后用padding去填充，基本上可以解决所有浏览器的问题

### 文字竖排排列显示方案
1.writing-mode:lr-tb或writing-mode:tb-rl
  参数：
	<1>、lr-tb：从左向右，从上往下
	<2>、tb-rl：从上往下，从右向左
	运行代码发现，IE显示正常，火狐、谷歌浏览器却不支持，所以不建议使用writing-mode属性。
2.对文字对象宽度设置只能排下一个文字的宽度距离，让文字一行排不下两个文字使其文字自动换行，就形成了竖立排版需求。
3.利用html br换行标签实现文字换行，对每个文字后加上换行br标签让其竖列排版。