---
title: html常见问题
date: 2016-11-25 09:45:55
tags: html
categories: html
---

### 标签变为可编辑
> contenteditable=true

### textarea标签问题
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
var storage = JSON.parse(window.localStorage.getItem("Data")) || {};  //从本地取localstorage数据
storage["typeId"] = id;    //localstorage数据设置key = value;
window.localStorage.setItem('Data',JSON.stringify(storage));   // 设置好的数据存到localstorage
JSON.parse()将字符串转换为对象
JSON.stringify()将对象转换为字符串
```


### a标签href属性多功能
```
1、<a href="tel:400-888-6633">拨打电话<a>
2、<a href="sms:19956321564">发送短信<a>
3、<a href="mailto:mail@mail.com">发送邮件<a>
```

### a标签rel
```
<a rel="nofollow noopener noreferrer">
```
使用a标签的target="_blank"属性，或者window.open(url)在新窗口中打开页面时，会存在潜在的安全问题。为什么呢？这个锅是一个叫opener的全局对象的锅。
通过在a标签上添加这个noopener属性，可以将新打开窗口的opner置为空
可解决除IE外的安全问题，和所有现代浏览器的性能问题

### 利用css3 @media 媒体查询方法实现 引用不同link标签
```html
    <!-- 使用： -->
    <!-- mediatype: all || print || screen || speech -->
    <link rel="stylesheet" media="mediatype and|not|only (media feature)" href="mystylesheet.css">
    <!-- 具体示例: -->
    <link rel="stylesheet" media="screen and (max-width:750px)" href="./css/index.css">
    <link rel="stylesheet" media="screen and (min-width:750px)" href="./css/indexPC.css">
```

### 利用meta标签在Safari浏览器显示下载app tab

```html
<meta name="apple-itunes-app" content="app-id=xxxxx, app-argument=appname://feed">
```

### 文案中的引号和字段外的引号冲突解决
```js
//用反斜杠\包裹引号
const str = 'BIG SHOTS\'\ COMMENTS';
```

### js中并不能直接使用`↵`换行，可以替换为`\n`

### video标签autoplay无法自动播放
```html
<video width="550" autoplay muted playsinline loop>
    <source src="video.mp4" type="video/mp4" />
</video>
```
> playsinline：一个布尔属性，指明视频将内嵌（inline）播放，即在元素的播放区域内。请注意，没有此属性并不意味着视频始终是全屏播放的。
> muted：一个布尔属性，指明在视频中音频的默认设置。设置后，音频会初始化为静音。默认值是 false, 意味着视频播放的时候音频也会播放。（在chorme里不设置muted:true，autoplay不会生效）
> loop：一个布尔属性；指定后会在视频播放结束的时候，自动返回视频开始的地方，继续播放。