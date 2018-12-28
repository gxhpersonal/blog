---
title: css-mask
date: 2018-12-28 15:19:20
tags: other
categories: other
---
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
background: black url("http://www.kkkk1000.com/images/1534750163.jpg");
-webkit-mask-image: url("http://www.kkkk1000.com/images/1534750222.jpg");
animation: mask 5s linear infinite forwards;
}
</style>
<div class="mask"> </div>