---
title: html常见问题
date: 2016-11-25 09:45:55
tags: html
categories: html
---

### 标签变为可编辑
> contenteditable=true

###textarea标签问题
加边框在移动端会出现双边框

### img标签中的图片无法显示
1、让这个图片元素隐藏：
onerror="this.style.display='none'"/>
2、用默认的图片替换：
* 控制onerror事件只触发一次，需要增加这句话：this.onerror=null;
onerror="this.src='默认图片的url地址;this.onerror=null'"/>

### input标签在chrome浏览器下背景颜色变黄色的问题
如图
![](http://ota5i8p1g.bkt.clouddn.com/input.png)

解决方法：
>方法一：
```
 <input type="text"  autocomplete="off">，没错，就是给input标签设置禁用自动完成属性；出现黄色背景就是因为启用了自动完成属性；
```
>方法二：
 -webkit-box-shadow: 0 0 0px 1000px white inset；  没错，就是给input设置内置阴影！而且一定要大，至少要比你的input本身大！不过，box-shadow是很慢的！而且，如果你的input是用图片做背景or，是没有办法做这么干的！


### localstorage
```javascript
var storage = JSON.parse(window.localStorage.getItem("CdlformData")) || {};  //从本地取localstorage数据
storage["DistributionType"] = id;    //localstorage数据设置key = value;
window.localStorage.setItem('CdlformData',  JSON.stringify(storage));   // 设置好的数据存到localstorage
```

### h5 video标签
>自定义播放/暂停,放大，缩小按钮
```html
<!DOCTYPE html> 
<html> 
<head> 
<meta charset="utf-8"> 
<title>h5 vide</title> 
</head>
<body> 

<div style="text-align:center"> 
  <button onclick="playPause()">播放/暂停</button> 
  <button onclick="makeBig()">放大</button>
  <button onclick="makeSmall()">缩小</button>
  <button onclick="makeNormal()">普通</button>
  <br> 
  <video id="video1" width="420">
    <source src="http://huizuche.qiniudn.com/video/USA.mp4" type="video/mp4">
    您的浏览器不支持 HTML5 video 标签。
  </video>
</div> 

<script> 
var myVideo=document.getElementById("video1"); 

function playPause()
{ 
	if (myVideo.paused) 
	  myVideo.play(); 
	else 
	  myVideo.pause(); 
} 

	function makeBig()
{ 
	myVideo.width=560; 
} 

	function makeSmall()
{ 
	myVideo.width=320; 
} 

	function makeNormal()
{ 
	myVideo.width=420; 
} 
</script> 

</body> 
</html>
```