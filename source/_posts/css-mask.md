---
title: css-mask
date: 2018-12-28 15:19:20
tags: other
categories: other
---
> mask-position: 0px 0px;//设置遮罩层的位置
> mask-size: 100%;//设置遮罩层的大小
> mask-image：url() //设置遮罩层的图像。
<style>
@keyframes mask {
0% {
-webkit-mask-position: 0px 0px;
}

25% {
-webkit-mask-position: 619px 0px;
}

50% {
-webkit-mask-position: 0px 0px;
}

75% {
-webkit-mask-position: 308px 0px;
-webkit-mask-size: 100%;
}

100% {
-webkit-mask-size: 1000%;
}
}

.mask {
width: 700px;
height: 392px;
background: black url("http://www.guoxh.com/blog/img/blog/哈利波特1.jpg");
-webkit-mask-image: url("http://www.guoxh.com/blog/img/blog/哈利波特2.jpeg");
animation: mask 5s linear infinite forwards;
}
</style>

<body>
<div class="mask"> </div>
</body>
