---
title: css3 animate
date: 2016-11-22 09:35:57
tags: css
categories: css
---

## animation:
* animation-name : xuanzhuan  //动画名字
* animation-duration : 1s ;   //动画时长
* animation-timing-function : linear; //动画运动方式
* animation-fill-mode : both;  //动画最终停留状态
* animation-iteration-count : infinite; //动画运动次数
* animation-direction : alternate; //动画来回
* animation-delay : 1s;   //动画延迟
* animation-play-state : paused  / running;  //给js 用的动画控制

$.fn.fullpage.  调用方法

## 旋转图片：transform:    
*  旋转：rotateY  rotateZ  rotateX();          (deg)  
*  放大：scaleX scaleY scaleZ scale scale3d          (1.1)  
*  斜切：skewX skewY skewZ skew3d          (deg)
*  位移：translateX translateY  translateZ  translate3d (px)

## 背景图片颜色渐变：
   background-image:linear-gradient(#33bb11,#ffaacc)
   < linear-gradient>：使用线性渐变创建背景图像。
   < radial-gradient>：使用径向(放射性)渐变创建背景图像。
   < repeating-linear-gradient>：使用重复的线性渐变创建背景图像。
   < repeating-radial-gradient>：使用重复的径向(放射性)渐变创建背景图像

## 定义一个动画的方法
* @keyframes

## transform-origin:top center;

## 永远不要写all 写那些真正需要监测的属性
* ease ease-in ease-in-out ease-out ease-out linear step-end step-start steps()
* transition-property: all;
* transition-duration: .8s;
* transition-timing-function: ease;
* transition-delay: 0;

## animation-timing-function:cubic-bezier();
      both:让动画留在最后一帧；
      无限动画：infinite

// 进入一屏的时候会调用afterLoad
afterLoad:function(name,index){
  name是当前这张的名字 index 当前是第几张
}
// 离开一屏的时候 会调用onLeave
onLeave:function(index,next,dir){
  if(index === 5){
    $('#section5 h1').addClass('animate-fei');
  }
  if(index === 1){
    //离开失效
    return false;
  }
}
## 四、使用css结合js去制作页面中的动画

1、使用 transtion 结合fullpage 去制作动画
```
transition:transform .5s ease 1s ,opacity .6s ease-in-out;
transform:rotateX()
transform:translate3d(30px,40px,0)
```
2、使用 animation 结合 fullpage 去制作动画

a.要动的元素写到每一个section中
b.正常状态下采用一种动画(可选)
c.在section拥有active类的情况下彩另外一种动画
d.一些复杂的情况，把动画预先写好， 在配置项的onLeave afterLoad回调函数中通过添加类名

<canvas id="canvas" style="position:fixed;width:100%;height:100%;background:#000;" width="1920" height="1080"></canvas>
<script>
var can = document.getElementById('canvas');
var cxt = can.getContext('2d');
can.width = window.screen.width;
can.height = window.screen.height;
var size = 14;
var cols = can.width / size; 
var y = [];
for (var i = 0; i < cols; i++)y[i] = 0;
(function draw() {
cxt.fillStyle = 'rgba(0,0,0,.1)';
cxt.fillRect(0, 0, can.width, can.height);
cxt.fillStyle = '#33ff00';
cxt.font = 'bold ' + size + 'px Microsoft yahei';
for (var i = 0; i < cols; i++)
{
var s = Math.floor(Math.random() * 10) + ''; 
var textX = i * size; 
var textY = y[i];
cxt.fillText(s, textX, textY); 
y[i] += size; 
if (y[i] >= can.height && Math.random() >= 0.9) {
y[i] = 0;
};
}
requestAnimFrame(draw);
})()		  		
</script>