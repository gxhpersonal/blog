---
title: html常见问题
date: 2016-11-25 09:45:55
tags:
categories: html
---

### 标签变为可编辑
> contenteditable=true

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