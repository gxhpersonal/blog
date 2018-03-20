---
title: swiper插件
date: 2016-09-19 14:50:11
tags: swiper
categories: swiper
---

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
  </script>
</body>
</html>
```