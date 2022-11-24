---
title: swiper插件
date: 2016-09-19 14:50:11
tags: swiper
categories: swiper
---

### vue中使用swiper
```html
<!--！！！！重中之重：swiper不能用v-show或者v-if来控制显示隐藏，这样初始化时不会添加swiper-slide-active这个类名，没有这个类名，就无法动态控制显示哪一个slide,也就是参数initialSlide，用visibility: hidden来控制显示隐藏-->
<!--下面这个swiper实现了fixed布局下的swiper-->
<section class="swiper-box" :class="{'hidden':!showEqSwiper}">
      <div class="swiper-container">
        <div class="swiper-wrapper">
          <div class="swiper-slide" v-for="(v,i) in Data" :key="i">
            <img :src="v.image_url" alt>
            <h4>{{v.title}}</h4>
            <p>{{v.description}}</p>
          </div>
        </div>
      </div>
      <!--关闭按钮-->
      <img class="close-swiper" src="./img/eq-close.png" alt @click="showEqSwiper = false">
    </section>
```
```js
`$ npm install swiper`
//这种引入方式有bug，再微信PC端浏览器打不开，在部分老机型也打不开，比如iPhone6：
import Swiper from "swiper";
//另一种就是直接引入cdn文件（放在index.html中，当然这种是全局的）：
//而且如果想下载swiper.js到本地，还是会报错。。。暂时没找到原因；
<script src="https://cdnjs.cloudflare.com/ajax/libs/Swiper/4.5.0/js/swiper.min.js"></script>
export default {
mounted() {
    //轮播
    new Swiper(".swiper-container", {
      observer: true,
      //将observe应用于Swiper的父元素。当Swiper的父元素变化时，例如window.resize，Swiper更新
      observeParents: true,
      //centeredSlides设定为true时，active slide会居中，而不是默认状态下的居左。
      centeredSlides: true,
      //设置slider容器能够同时显示的slides数量(carousel模式)。默认：1
      slidesPerView: "auto",
      //分页器使用，如果要自定义样式，style不能加scope属性！！！！！！！！！重中之重。。。让我搞半天
      pagination: {
        el: ".prepay-swiper-pagination"
      }
    });
  },
  methods: {
    //这个方法用来控制 swiper显示并且切换到指定slide
    EqSwiper(v) {
      let that = this;
      let curIndex = v;
      this.showEqSwiper = true;
      new Swiper(".swiper-container", {
        centeredSlides: true,
        slidesPerView: "auto",
        //initialSlide设定初始化时slide的索引。
        initialSlide: curIndex
      });
    }
  }
}

```
```css
.swiper-box {
  display: flex;
  flex-direction: column;
  justify-content: center;
  position: fixed;
  top: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.7);
  width: 100%;
  height: 100%;
  &.hidden {
    visibility: hidden;
  }
  .swiper-container {
    padding: 0 1.38rem;
    .swiper-wrapper {
      display: flex;
      transition-property: transform;
    }
    .swiper-slide {
      width: 16rem;
      display: flex;
      flex-shrink: 0;
      flex-direction: column;
      align-items: center;
      background: #fff;
      border-radius: 0.38rem;
      margin: 0 0.35rem;
      img {
        border-radius: 0.38rem 0.38rem 0 0;
      }
      h4 {
        font-size: 1rem;
        font-weight: bold;
        color: rgba(34, 34, 34, 1);
        margin-top: 1.9rem;
      }
      p {
        font-size: 0.7rem;
        font-weight: bold;
        color: rgba(102, 102, 102, 1);
        margin-top: 0.63rem;
        padding-bottom: 1.7rem;
      }
    }
  }
  .close-swiper {
    width: 0.72rem;
    margin-top: 1.75rem;
    margin-left: 9rem;
  }
}
```

### 移动端单文件中使用swiper
需求环境：移动端环境，swiper.css, swiper.js, 如果用jquery的话，可以用jquery.js和swiper.jquery.js
以前也用过swiper做轮播，但是好多参数都不清楚，所以在这总结一下,有新的发现会更新

* 动态获取接口数据来轮播有问题，loop为true时，循环滚动第一帧会被跳过，暂时解决方案是加setTimeout，vue环境，可以用nextTick
*  Vue.nextTick(function () {
*    // DOM 更新了
*  })
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
    <meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no">
	<title>Document</title>
    <link href="https://cdn.bootcss.com/Swiper/4.0.1/css/swiper.css" rel="stylesheet">
</head>

<body>
  <style>
    .swiper-container {
    width: 100%;
    height: 300px;
}
  </style>

   <div class="swiper-container">
    <div class="swiper-wrapper">
        <div class="swiper-slide">Slide 1</div>
        <div class="swiper-slide">Slide 2</div>
        <div class="swiper-slide">Slide 3</div>
		<div class="swiper-slide">Slide 4</div>
        <div class="swiper-slide">Slide 5</div>
    </div>
    <!-- 如果需要分页器 -->
    <div class="swiper-pagination"></div>
    
    <!-- 如果需要导航按钮 -->
    <div class="swiper-button-prev"></div>
    <div class="swiper-button-next"></div>
    
    <!-- 如果需要滚动条 -->
    <!-- <div class="swiper-scrollbar"></div> -->
</div>
    <!-- 导航等组件可以放在container之外 -->

   <script src="https://cdn.bootcss.com/jquery/3.2.0/jquery.js"></script>
   <script src="https://cdn.bootcss.com/Swiper/3.3.0/js/swiper.js"></script>
   <script>
  var mySwiper = new Swiper ('.swiper-container', {
    // direction: 'vertical',
    loop: true,
    autoplay: 3000,
    //如果需要小圆点可点击
    paginationClickable: true,
    //如果小圆点要变成数字(分式)，例如 1/3
    // paginationType: 'fraction',
    // 如果需要分页器
    pagination: '.swiper-pagination',
    
    // 如果需要前进后退按钮
    nextButton: '.swiper-button-next',
    prevButton: '.swiper-button-prev',
    //滑动后停止轮播问题：
    autoplayDisableOnInteraction : false,
    // 如果需要滚动条
    // scrollbar: '.swiper-scrollbar',
  })
//
//一张图停止轮播，如果只有一个slide就销毁swiper //因为loop为true时会在图片前后各加一个slide所以最少为3
        if (swiper.slides.length <= 3) {
          swiper.destroy();
        }
  </script>
</body>
</html>
```