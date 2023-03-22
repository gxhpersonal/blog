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
@keyframes clipRectSpIn {
    0%   {
        clip-path: polygon(50% 20%, 50% 50%, 20% 50%, 50% 50%, 50% 80%, 50% 50%, 80% 50%, 50% 50%);
    }
    50% {
        clip-path: polygon(50% 0%, 0% 0%, 0% 50%, 0% 100%, 50% 100%, 100% 100%, 100% 50%, 100% 0%);
    }
    100%   {
        clip-path: polygon(50% 20%, 50% 50%, 20% 50%, 50% 50%, 50% 80%, 50% 50%, 80% 50%, 50% 50%);
    }
}
.box{
    width: 650px;
    height: 392px;
    position:relative;
}
.mask {
    width: 100%;
    height: 100%;
    background:url("http://www.guoxh.com/blog/img/blog/halibote2.webp") no-repeat left top;
    background-size: cover;
    position: absolute;
    left: 0;
    top: 0;
}
.bg{
    width: 100%;
    height: 100%;
    -webkit-mask: url("http://www.guoxh.com/blog/img/blog/halibote1.jpeg"); 
    -webkit-mask-size: 3000% 100%;
    animation: maskMove 2s steps(29) infinite;
    position: absolute;
    left: 0;
    top: 0;
}
.mask::before{
    content:"";
    display:block;
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: url("http://www.guoxh.com/blog/img/blog/halibote1.jpeg") no-repeat left top;
    background-size: cover;
    animation: clipRectSpIn 6s infinite;
}
/* 这是分割线 */
/* VS效果 */
.mask2{
    width:650px;
    height:270px;
    position:relative;
    background:url("http://www.guoxh.com/blog/img/blog/halibote4.webp") no-repeat;
    background-size:100% 100%;
    background-position:150px;
    margin-top:30px;
}
.mask2::before{
    content:"";
    display:block;
    width:650px;
    height:270px;
    position:absolute;
    top: 0;left: 0; right: 0;bottom: 0;
    background: url("http://www.guoxh.com/blog/img/blog/halibote5.png");
    background-position:-180px;
    -webkit-mask: linear-gradient(45deg, #000 45%, transparent 55%);
}
</style>

<body>
<div class="box">
<div class="mask"></div>
<!-- <div class="bg"></div> -->
</div>

<div class="mask2"></div>
</body>
