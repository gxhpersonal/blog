---
title: scratchers
date: 2019-08-29 16:09:21
tags: other
categories: other
---

<style>
    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
    }

    body {
        width: 100%;
        height: 100%;
        background: #dfdfdf;
        text-align: center;
    }

    .canvas-only-container h1 {
        padding-top: 50px;
    }

    .canvas-only-container #result {
        padding-top: 50px;
        color: red;
    }

    #canvasOnly {
        width: 240px;
        height: 137px;
        background: url('http://www.guoxh.com/blog/img/blog/special.jpg');
        background-size: 100% 100%;
        margin: 0 auto;
        margin-top: 50px;
    }
</style>

<body>
    <div class="canvas-only-container">
        <h1>刮奖</h1>
        <div id="canvasOnly"><canvas id="maskOnlyOne" width="240" height="137"></canvas></div>
        <h3 id="result"></h3>
    </div>
</body>

<script>
    var canvas = document.getElementById('maskOnlyOne');
    var context = canvas.getContext("2d");
    context.fillStyle = "#d1d1d1";
    context.fillRect(0, 0, 240, 137);
    context.globalCompositeOperation = 'destination-out';
    // 鼠标按下 增加mousemove的事件监听
    canvas.addEventListener('mousedown', drawArcMouseHandle);
    canvas.addEventListener('mouseup', function (event) {
        // 鼠标抬起之后，把mousemove的事件监听撤销掉
        this.removeEventListener('mousemove', mousemoveHandle);
    });
    // 根据鼠标的move画圆
    function drawArcMouseHandle(event) {
        event.preventDefault();
        event.target.addEventListener("mousemove", mousemoveHandle);
    }
    // 为了能够移除movesemove的事件需要单独处理一下
    function mousemoveHandle(event) {
        event.preventDefault();
        drawArcByPoint(event.pageX, event.pageY);
    }
    // 监听 touchmove
    canvas.addEventListener('touchmove', drawArcTouchHandle);
    // 根据触摸点画圆 
    function drawArcTouchHandle(event) {
        event.preventDefault();
        var touch = event.touches[0];
        drawArcByPoint(touch.pageX, touch.pageY);
    }
    // 根据某个点在canvas上画圆
    // x 坐标和 y 坐标 两个坐标是触摸点的坐标而不是画圆的圆心
    // 圆心通过计算得出
    function drawArcByPoint(x, y) {
        context.beginPath();
        context.arc(x - canvas.offsetLeft, y - canvas.offsetTop, 20, 0, Math.PI * 2);
        context.closePath();
        context.fillStyle = '#dddddd';
        context.fill();
        checkComplete();
    }
    // 判断是否完成刮奖 点数大于80%
    function checkComplete() {
        var imgData = context.getImageData(0, 0, 240, 137);
        var pxData = imgData.data; // 获取字节数据
        var len = pxData.length; // 获取字节长度
        var count = 0; // 记录透明点的个数
        // 主要的思想是 一个像素由四个数据组成，每个数据分别是 rgba() 所以第四个数据 a 表示alpha透明度
        for (var i = 0; i < len; i += 4) {
            var alpha = pxData[i + 3]; // 获取每个像素的透明度
            if (alpha < 10) {
                // 透明度小于10 
                count++;
            }
        }
        var percent = count / (len / 4); // 计算百分比
        // 如果百分比大于0.8 则表示成功
        if (percent >= 0.8) {
            showResult();
        }
    }
    // 显示刮奖结果
    function showResult(msg) {
        msg = msg || "刮奖结束";
        var res = document.getElementById('result');
        res.innerHTML = msg;
    }
</script>