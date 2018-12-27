---
title: 其他css问题
date: 2016-09-14 18:07:27
tags: css
categories: css
---

### flex布局	
[更多参见阮老师blog](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html?utm_source=tuicool)
1.意为弹性布局，块元素：display:flex;行内元素：display:inline-flex;
2.不同内核浏览器需要加前缀区分，如：display:-webkit-flex; /*Safari*/  `非规范属性值：display:-webkit-box`
3.设为flex布局后，子元素的float,clear,vertical-align属性将失效。  `非规范属性：-webkit-box-orient：vertical //垂直排列子元素`
4.flex属性：
1）flex-direction:row（默认值）：主轴为水平方向，起点在左端。
                  row-reverse：主轴为水平方向，起点在右端。
                  column：主轴为垂直方向，起点在上沿。
                  column-reverse：主轴为垂直方向，起点在下沿
2)flex-wrap属性: nowrap（默认）：不换行。
                wrap：换行，第一行在上方。
                wrap-reverse：换行，第一行在下方。
3)flex-flow：属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap。
4)justify-content:flex-start | flex-end | center | space-between | space-around
5)align-items: flex-start | flex-end | center | baseline | stretch;

### 清除所有a标签在点击时出现的特效
> 清除所有a标签在点击时出现的特效：a{ -webkit-tap-highlight-color:rgba(255,0,0,0);}

### 文字与图片不能垂直居中对齐
> 给图片添加css样式
vertical-align:text-bottom；
vertical-align只适用于行内块元素

### css实现禁止点击事件
> pointer-events: none;
> 阻止点击事件，变为默认光标，阻止触发hover，active，阻止JS点击事件

### img标签和同级div之间的间隙处理
> 给img标签设置 vertical-align:middle;display:block;

### 在移动端手机APP中禁止长按来阻止其他手机自带默认事件（如：iPhone的3Dtouch）
```
    touch-callout: none;
    -webkit-touch-callout: none;
    -webkit-user-select: none;
    user-select: none;
```

### 处理激活状态文字移动问题
```
margin-top:-1px;
margin-bottom:1px;
padding-top:1px;
```

### 文本超出后显示省略号
```
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
*line-clamp*
限制在一个块元素显示的文本的行数。
-webkit-line-clamp 是一个 不规范的属性（unsupported WebKit property），它没有出现在 CSS 规范草案中。
为了实现该效果，它需要组合其他外来的WebKit属性。常见结合属性：
display: -webkit-box; 必须结合的属性 ，将对象作为弹性伸缩盒子模型显示 。
-webkit-box-orient 必须结合的属性 ，设置或检索伸缩盒对象的子元素的排列方式 。
text-overflow，可以用来多行文本的情况下，用省略号“...”隐藏超出范围的文本 。
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

### ios下input样式问题
1.ios下如果想要禁用input,input设置为readonly仍然会呼起键盘，所以必须设置disabled属性。
2.ios下设置为disabled属性，input表单背景颜色会变灰，透明度会降低，所以，必须设置
```
	input:disabled{
       opacity:1;
       bakcground:#fff;
       -webkit-text-fill-color:#2e3d4c;//字体颜色填充这个属性可以解决IOS字体颜色变淡的问题
	}
然而，设置了-webkit-text-fill-color属性还是会有问题，placeholder字体颜色也会跟着改变，所以placeholder也要设置-webkit-text-fill-color值
题外话：（设置渐变字体）
	background: -webkit-linear-gradient(top,#fc0,#f30 50%,#c00 51%,#600);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
```

### -webkit-overflow-scrolling
> 用来控制元素在移动设备上是否使用滚动回弹效果.
> 兼容安卓和IOS的写法如下
```
-webkit-overflow-scrolling: touch; /* 当手指从触摸屏上移开，会保持一段时间的滚动 */ 
-webkit-overflow-scrolling: auto; /* 当手指从触摸屏上移开，滚动会立即停止 */ 
```
> Bug
>当你给一个元素设置过position:absolute;或者position:relative;后再增加-webkit-overflow-scrolling: touch;属性后，你会发现，滑动几次后可滚动区域会卡主，不能在滑动，这时给元素增加个z-index值就可以了。
```
-webkit-overflow-scrolling: touch; 
position:absolute; 
z-index:1; 
```

### css3滤镜效果：-webkit-filter
[http://www.css88.com/html5-demo/-webkit-filter/index.html]()

### 字体抗锯齿效果
Webkit在自己的引擎中支持了这一效果。
-webkit-font-smoothing
它有三个属性值：
none ------ 对低像素的文本比较好
subpixel-antialiased------默认值
antialiased ------抗锯齿很好 
.icon {

-webkit-font-smoothing: antialiased;

-moz-osx-font-smoothing: grayscale;  //Gecko也推出了自己的抗锯齿效果的非标定义

}

### 弹层出现禁用背景body滚动条
height:100%;overscroll:hidden;
touch-action:none;//禁止触发默认的手势操作。
pan-x：可以在父级元素(the nearest ancestor)内进行水平移动的手势操作。
pan-y：可以在父级元素内进行垂直移动的手势操作。
manipulation：允许手势水平/垂直平移或持续的缩放。任何auto属性支持的额外操作都不支持；
注：touch-action只支持具有行内块，块级的元素

### CSS控制输入文本大小写
> text-transform：none；capitalize；uppercase；lowercase；inherit；
>none默认。定义带有小写字母和大写字母的标准的文本。
capitalize文本中的每个单词以大写字母开头。
uppercase定义仅有大写字母。lowercase定义无大写字母，仅有小写字母。
inherit规定应该从父元素继承 text-transform 属性的值。

### 控制input标签的placeholder属性实现获取焦点显示暗文，失去焦点隐藏(其实就是改变placeholder中的文字颜色)
```
    &::-webkit-input-placeholder { color:transparent; }
    &:-moz-placeholder { color:transparent; }
    &::-moz-placeholder { color:transparent; }
    &:-ms-input-placeholder { color:transparent;}
    &:focus::-webkit-input-placeholder { color:#9fb0bf; }
    &:focus:-moz-placeholder { color:#9fb0bf; }
    &:focus::-moz-placeholder { color:#9fb0bf; }
    &:focus:-ms-input-placeholder { color:#9fb0bf; }
```

### will-change提高页面滚动、动画等渲染性能css3
参考张鑫旭blog:
[http://www.zhangxinxu.com/wordpress/2015/11/css3-will-change-improve-paint/]()

### 支持webkit内核浏览器的滚动条样式自定义
>::-webkit-scrollbar{/* 1 */} /*滚动条垂直方向的宽度与水平方向的高度*/ ::-webkit-scrollbar-button{/* 2 */} /*滚动条按钮*/ ::-webkit-scrollbar-track{/* 3 */} /*滚动条轨道*/ ::-webkit-scrollbar-track-piece{/* 4 */} /*滚动条垂直方向轨道件*/ ::-webkit-scrollbar-thumb{/* 5 */} /*滚动条轨道上的按钮*/ ::-webkit-scrollbar-corner{/* 6 */} /*滚动条轨道上的滚动角*/ 

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

### 页面加滤镜
```
//高斯模糊
filter: blur(px)；
//转换为灰度图像
filter: grayscale(100%);
-webkit-filter: grayscale(100%);
-moz-filter: grayscale(100%);
-ms-filter: grayscale(100%);
-o-filter: grayscale(100%);
filter: progid:DXImageTransform.Microsoft.BasicImage(grayscale=1);
-webkit-filter: grayscale(1);    
```

### mask
css的mask属性允许使用者通过部分或者完全隐藏一个元素的可见区域。这种效果可以通过遮罩或者裁切特定区域的图片。

### clip 图像裁剪
clip:rect(0px,0px,0px,0px)四个值分别对应上右下左

### 滚动动画
html, body { scroll-behavior:smooth; }
凡是需要滚动的地方都加一句scroll-behavior:smooth就好了！
如：<a href="#">返回顶部</a>
> js处理：
> target.scrollIntoView({
    behavior: "smooth"
});
我们随便打开一个有链接的页面，把首个链接滚动到屏幕外，然后控制台输入类似下面代码，我们就可以看到页面平滑滚动定位了：

document.links[0].scrollIntoView({
    behavior: "smooth"
});

### box-shadow添加多个不同的阴影
> 用逗号分隔即可
> 如：
> box-shadow: 0 3px 3px 0 rgba(243,132,0, 0.35), 0 -6px 2px 0 rgba(244, 149, 0,0.35) inset;

### 背景色渐变
/* 渐变轴为45度，从蓝色渐变到红色 */
linear-gradient(45deg, blue, red);

/* 从右下到左上、从蓝色渐变到红色 */
linear-gradient(to left top, blue, red);

/* 从下到上，从蓝色开始渐变、到高度40%位置是绿色渐变开始、最后以红色结束 */
linear-gradient(0deg, blue, green 40%, red);

### iphoenX适配问题(利用media限制iPhone X屏幕尺寸)
```
// css部分，利用media做你想要的适配，我这里通过一个白色div留出安全距离防挡
@media only screen and (device-width: 375px) and (device-height: 812px) and (-webkit-device-pixel-ratio: 3) {
  .adapt {
    position: fixed;
    bottom: 0;
    width: 100%;
    height: 34px;
    background: #fff;
  }
}
```

### position:sticky
1.父级元素不能有任何overflow:visible以为的overflow设置，否则没有粘滞效果。因为改变了滚动容器（即使没有出现滚动条）。因此，如果你的position:sticky无效，看看是不是某一个祖先元素设置了overflow:hidden，移除之即可。
2.同一个父容器中的sticky元素，如果定位值相等，则会重叠；如果属于不同父元素，则会鸠占鹊巢，挤开原来的元素，形成依次占位的效果。
3.sticky定位，不仅可以设置top，基于滚动容器上边缘定位；还可以设置bottom，也就是相对底部粘滞。如果是水平滚动，也可以设置left和right值。