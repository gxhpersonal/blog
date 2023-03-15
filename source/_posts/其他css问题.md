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
6)当使用了`flex-direction:column`之后,未定宽的元素会自动适应宽度为100%；给元素加个非默认值`align-self:stretch`外的值就可以

### grid布局
grid布局比起flex布局更加灵活，flex布局只有横轴纵轴概念，grid布局则多了个网格概念，可以控制每个单元格样式


### 清除所有a标签在点击时出现的特效
> 清除所有a标签在点击时出现的特效：a{ -webkit-tap-highlight-color:rgba(255,0,0,0);}

### 文字与图片不能垂直居中对齐
> 给图片添加css样式
vertical-align:text-bottom；
vertical-align只适用于行内块元素

### css实现禁止点击事件
> pointer-events: none;
> 阻止点击事件，变为默认光标，阻止触发hover，active，阻止JS点击事件
> none:
元素永远不会成为鼠标事件的target。但是，当其后代元素的pointer-events属性指定其他值时，鼠标事件可以指向后代元素，在这种情况下，鼠标事件将在捕获或冒泡阶段触发父元素的事件侦听器。
* 扩展：如果有一个公共组件，里面有个可移动元素，父元素设置`pointer-events: none;`，并且子元素设置`pointer-events: auto;`，可以实现子元素移动同时父元素不可点击并且不影响被父元素遮盖的元素点击事件，比如小程序里可拖动组件如果全屏拖动必须area标签设置全屏，然后下面的元素就无法选中。

### img标签和同级div之间的间隙处理
> 给img标签设置 vertical-align:middle;display:block;

### 在移动端手机APP中禁止长按来阻止其他手机自带默认事件（如：iPhone的3Dtouch）
```css
    touch-callout: none; //当你触摸并按住触摸目标时候，禁止或显示系统默认菜单
    -webkit-touch-callout: none;
    -webkit-user-select: none; //用户能否选中文本
    user-select: none;
```

### 文本超出后显示省略号
```css
/* 一行 */
p{
   width:40px;
   white-space: nowrap;
   text-overflow: ellipsis;
   overflow: hidden;
}
/* 多行 */
p{
    overflow: hidden;                /* 超出部分隐藏 */
    text-overflow: ellipsis;         /* 显示省略号来修饰被剪掉的文本 */
    display: box;
    display: -webkit-box;
    line-clamp: 3;                     /* 非规范属性，实现规定文本行数      火狐不支持这个属性 */
    -webkit-line-clamp: 3;
    -webkit-box-orient: vertical;    /* 规定框的子元素应该被水平或垂直排列 */
    -moz-box-orient: vertical;        /* 支持火狐的写法 */
} 
*line-clamp*
限制在一个块元素显示的文本的行数。
-webkit-line-clamp 是一个 不规范的属性（unsupported WebKit property），它没有出现在 CSS 规范草案中。
为了实现该效果，它需要组合其他外来的WebKit属性。常见结合属性：
display: -webkit-box; 必须结合的属性 ，将对象作为弹性伸缩盒子模型显示 。
-webkit-box-orient 必须结合的属性 ，设置或检索伸缩盒对象的子元素的排列方式 。
text-overflow，可以用来多行文本的情况下，用省略号“...”隐藏超出范围的文本 。
```

### 自定义鼠标指针
> cursor: url(/路径/cursor.cur),auto;        后缀似乎必须为cur

### 文字竖排排列显示方案
1.writing-mode:lr-tb或writing-mode:tb-rl
  参数：
	<1>、lr-tb：从左向右，从上往下
	<2>、tb-rl：从上往下，从右向左
	运行代码发现，IE显示正常，火狐、谷歌浏览器却不支持，所以不建议使用writing-mode属性。
2.对文字对象宽度设置只能排下一个文字的宽度距离，让文字一行排不下两个文字使其文字自动换行，就形成了竖立排版需求。
3.利用html br换行标签实现文字换行，对每个文字后加上换行br标签让其竖列排版。

### -webkit-overflow-scrolling
> 用来控制元素在移动设备上是否使用滚动回弹效果.
> 兼容安卓和IOS的写法如下
```css
-webkit-overflow-scrolling: touch; /* 当手指从触摸屏上移开，会保持一段时间的滚动 */ 
-webkit-overflow-scrolling: auto; /* 当手指从触摸屏上移开，滚动会立即停止 */ 
```
> Bug
>当你给一个元素设置过position:absolute;或者position:relative;后再增加-webkit-overflow-scrolling: touch;属性后，你会发现，滑动几次后可滚动区域会卡主，不能在滑动，这时给元素增加个z-index值就可以了。
```css
-webkit-overflow-scrolling: touch; 
position:absolute; 
z-index:1; 
```

### overscroll-behavior可以控制只触发当前层的滚动效果
overscroll-behavior: auto | contain | none
> auto:（默认值），即滚动到边缘后继续滚动外部的可滚动容器
contain: 默认的滚动溢出行为只会表现在当前元素的内部（例如“反弹”效果或刷新），并且会阻止默认的滚动溢出行为
none:相邻的滚动区域不会发生滚动，并且会阻止默认的滚动溢出行为
contain和none的行为差异体现主要在移动端

### overflow-anchor属性改变滚动行为来控制可视区内容
overflow-anchor: auto | none
> auto:（默认值），即滚动时不会受内容变化影响。滚动条变化，可视内容不变
none:即滚动时会受内容变化影响，可视区插入内容会显示插入的内容。滚动条不变，可视内容变化

### css3滤镜效果：-webkit-filter
[https://developer.mozilla.org/zh-CN/docs/Web/CSS/filter]()

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

### will-change提高页面滚动、动画等渲染性能css3
参考张鑫旭blog:
[http://www.zhangxinxu.com/wordpress/2015/11/css3-will-change-improve-paint/]()

### 支持webkit内核浏览器的滚动条样式自定义
```css
::-webkit-scrollbar{width: 5px;height: 8px;background-color: #aaa} /*滚动条垂直方向的宽度与水平方向的高度*/ 
::-webkit-scrollbar-button{background-color: #aaa} /*滚动条按钮*/ 
::-webkit-scrollbar-track{background-color: #aaa} /*滚动条轨道*/ 
::-webkit-scrollbar-track-piece{background-color: #aaa} /*滚动条垂直方向轨道件*/ 
::-webkit-scrollbar-thumb{background-color: #aaa} /*滚动条轨道上的按钮*/ 
::-webkit-scrollbar-corner{background-color: #aaa} /*滚动条轨道上的滚动角*/ 
```

### 页面加滤镜
```css
/* 高斯模糊 */
filter: blur(px)；
/* 转换为灰度图像 */
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
mask-reference:设置遮罩图片的路径
masking-mode:设置遮罩图片的模式
position:设置遮罩图片的位置
bg-size:设置遮罩的大小
repeat-style:设置遮罩图片的重复性
compositing-operator:设置遮罩图层的组合操作
```css
.anothertarget {
  mask: url(resources.svg#c1) 50px 30px/10px 10px repeat-x exclude;
}
```

### clip 图像裁剪
```css
*{
  clip:rect(0px,0px,0px,0px) //四个值分别对应上右下左
}
```

### 滚动动画
html, body { scroll-behavior:smooth; }
凡是需要滚动的地方都加一句scroll-behavior:smooth就好了！
如：<a href="#">返回顶部</a>
> js处理：
> target.scrollIntoView({
    behavior: "smooth"
});
我们随便打开一个有链接的页面，把首个链接滚动到屏幕外，然后控制台输入类似下面代码，我们就可以看到页面平滑滚动定位了：
```js
document.links[0].scrollIntoView({
    behavior: "smooth"
});
```

### box-shadow添加多个不同的阴影
> 用逗号分隔即可
> 如：
> ```css
> *{box-shadow: 0 3px 3px 0 rgba(243,132,0, 0.35), 0 -6px 2px 0 rgba(244, 149, 0,0.35) inset;}
> ```

### 背景色渐变
```css
*{
/* 渐变轴为45度，从蓝色渐变到红色 */
background-image:linear-gradient(45deg, blue, red);

/* 从右下到左上、从蓝色渐变到红色 */
background-image:linear-gradient(to left top, blue, red);

/* 从下到上，从蓝色开始渐变、到高度40%位置是绿色渐变开始、最后以红色结束 */
background-image:linear-gradient(0deg, blue, green 40%, red);
}
```

### iphoenX适配问题(利用media限制iPhone X屏幕尺寸)
```css
/* css部分，利用media做你想要的适配，我这里通过一个白色div留出安全距离防挡 */
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

### css设置暗黑模式
```css
@media (prefers-color-scheme: dark) {
    body {
        color: white;
        background: black;
    }
}
```

### CSS all
> CSS `all` 简写属性 将除却 `unicode-bidi` 与 `direction` 之外的所有属性重设至其初始值，或继承值。
* 取值
initial
该关键字代表改变该元素或其父元素的所有属性至初始值。
inherit
该关键字代表改变该元素或其父元素的所有属性的值至他们的父元素属性的值。inherited values
unset
该关键字代表如果该元素的属性的值是可继承的，则改变该元素或该元素的父元素的所有属性的值为他们父元素的属性值，反之则改变为初始值。

### css优化
1. 加载性能：
（1）css压缩：将写好的css进行打包压缩，可以减少很多的体积。
（2）css单一样式：当需要下边距和左边距的时候，很多时候选择：`margin:top0 bottom0;`但`margin-bottom:bottom;margin-left:left;`执行的效率更高。
（3）减少使用@import,而建议使用link，因为后者在页面加载时一起加载，前者是等待页面加载完成之后再进行加载。

2. 选择器性能：
（1）关键选择器（keyselector）。选择器的最后面的部分为关键选择器（即用来匹配目标元素的部分）。CSS选择符是从右到
左进行匹配的。当使用后代选择器的时候，浏览器会遍历所有子元素来确定是否是指定的元素等等；
（2）如果规则拥有ID选择器作为其关键选择器，则不要为规则增加标签。过滤掉无关的规则（这样样式系统就不会浪费时间去匹
配它们了）。
（3）避免使用通配规则，如*{}计算次数惊人！只对需要用到的元素进行选择。
（4）尽量少的去对标签进行选择，而是用class。
（5）尽量少的去使用后代选择器，降低选择器的权重值。后代选择器的开销是最高的，尽量将选择器的深度降到最低，最高不要超过
三层，更多的使用类来关联每一个标签元素。
（6）了解哪些属性是可以通过继承而来的，然后避免对这些属性重复指定规则。

3. 渲染性能：
（1）慎重使用高性能属性：浮动、定位。
（2）尽量减少页面重排、重绘。
（3）去除空规则：｛｝。空规则的产生原因一般来说是为了预留样式。去除这些空规则无疑能减少css文档体积。
（4）属性值为0时，不加单位。
（5）属性值为浮动小数0.**，可以省略小数点之前的0。
（6）标准化各种浏览器前缀：带浏览器前缀的在前。标准属性在后。
（7）不使用@import前缀，它会影响css的加载速度。
（8）选择器优化嵌套，尽量避免层级过深。
（9）css雪碧图，同一页面相近部分的小图标，方便使用，减少页面的请求次数，但是同时图片本身会变大，使用时，优劣考虑清
楚，再使用。
（10）正确使用display的属性，由于display的作用，某些样式组合会无效，徒增样式体积的同时也影响解析性能。
（11）不滥用web字体。对于中文网站来说WebFonts可能很陌生，国外却很流行。webfonts通常体积庞大，而且一些浏
览器在下载webfonts时会阻塞页面渲染损伤性能。

4. 可维护性、健壮性：
（1）将具有相同属性的样式抽离出来，整合并通过class在页面中进行使用，提高css的可维护性。
（2）样式与内容分离：将css代码定义到外部css中。

### css设置图片`img`标签显示样式
`object-fit`属性
* 支持：chrome: >= 31.0; IE: >= 16.0; firefox: >= 36.0; safari: >= 7.1;
> 值	     描述 <br/>
> fill	   默认，不保证保持原有的比例，内容拉伸填充整个内容容器。 <br/>
> contain	 保持原有尺寸比例。内容被缩放。 <br/>
> cover	   保持原有尺寸比例。但部分内容可能被剪切。 <br/>
> none	   保留原有元素内容的长度和宽度，也就是说内容不会被重置。 <br/> 
> scale-down	保持原有尺寸比例。内容的尺寸与 none 或 contain 中的一个相同，取决于它们两个之间谁得到的对象尺寸会更小一些。 <br/>
> initial	 设置为默认值 <br/>
> inherit	 从该元素的父元素继承属性。

### 卡片翻转效果实现
```html
<style>
  .box{
    width: 50px;
    height: 50px;
    transition: 0.6s;
    transform-style: preserve-3d;/*子元素将保留其 3D 位置*/
  }
  .box.rotate {
    transform: rotateY(180deg);
  }
  .front{
    width: 50px;
    height: 50px;
    background: green;
    backface-visibility: hidden;/* 定义当元素不面向屏幕时是否可见，hidden：背面是不可见的 */
  }
  .back {
    backface-visibility: hidden;
    position: absolute;
    top: 0;
    left: 0;
    transform: rotateY(180deg);
    width: 50px;
    height: 50px;
    background: red;
  }
</style>
<div class="box">
  <div class="front"></div>
  <div class="back"></div>
</div>
```

### :empty 选择器
> `:empty`代表没有子元素的元素。子元素只可以是元素节点或文本（包括空格）。注释或处理指令都不会产生影响。
* 之前判断数据为空显示`暂无数据`需要判断数据length为0显示一个包裹暂无数据文案的标签，而用了`:empty`伪类选择器，只需要在外层div加一个伪类就可以实现；
```css
.container {
    width: 400px;
    height: 400px;
    background-color: skyblue;
    display: flex;
    justify-content: center;
    align-items: center;
}
.container:empty::after {
    content: "—— 没有更多了 ——";
}
```

### gap属性
当我们需要每个子元素之间距离等分而又除不尽时，就可以用到`gap`属性，`gap`属性它适用于`Grid`布局、`Flex`布局以及多列布局，并不一定只是`Grid`布局中可以使用。
比如我们要让每个元素之间隔开`20px`， 那么使用`gap`我们可以这样：
```css
.wrap{
  display: flex | grid；
  gap: 20px;
}
```

### background-clip 实现文本背景图
```css
.text{
  background-clip: text;
  -webkit-background-clip: text;
  color: rgba(0, 0, 0, 0.2);
}
```


### :invalid 伪类

### :focus-within 伪类

### mix-blend-mode:difference 

> `mix-blend-mode`属性描述了元素的内容应该与元素的直系父元素的内容和元素的背景如何混合。其中，difference 表示差值。

### `*-line-start`和`*-block-end`相关属性
其中 inline 和 block 表示方向， start 和 end 表示起止方位。

在中文和英文网页环境中，inline元素（文字、图片、按钮等）的默认是从左往右水平排列的；block元素（如`<div>`、`<p>`元素等）默认是从上往下垂直排列的。

因此，`margin-inline-start`就表示内联元素排列方向的起始位置，就是“左侧”，`margin-inline-end`就表示内联元素排列方向的终止位置，就是“右侧”；`margin-block-start`就表示块级元素排列方向的起始位置，就是“上面”，`margin-block-end`就表示块级元素排列方向的终止位置，就是“下面”。

### flex 布局 子元素不设置宽高，高度撑满父元素的问题
在 `flex` 布局中，我们通过 `align-items` 来控制元素在交叉轴上的对齐方式。

它可能取5个值：
flex-start: 交叉轴的起点对齐
flex-end: 交叉轴的终点对齐
center: 交叉轴的中点对齐。
baseline: 项目的第一行文字的基线对齐。
stretch (默认值）: 如果子元素未设置高度或者高度为auto，将占满整个容器的高度。

当我们没有给子元素增加高度的时候，其在交叉轴方向的对齐方式就是默认值 `stretch`，因此他的高度与父元素的高度一致。
所以如果想要自适应高度，子元素需单独设置`align-self:flex-start`，或者父元素不设置高度，靠子元素撑开;