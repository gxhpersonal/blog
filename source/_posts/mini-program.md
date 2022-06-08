---
title: 微信小程序记录
date: 2019-03-05 14:50:11
tags: 微信小程序
categories: 微信小程序
---

### 生命周期执行顺序：onLoad > onShow > onReady

### 禁止下拉
app.json中window下加enablePullDownRefresh:false

### 摇一摇实现相应操作
```js
onShow: function() {
    //重力加速度
    wx.onAccelerometerChange(function (res) {
        //console.log(res)
        //可以自定义大小，来决定摇晃什么程度触发方法
        if (res.x > .7 && res.y > .7) {
            wx.showToast({
                title: '摇一摇成功',
                icon: 'success',
                duration: 2000
            })
        }
    })
}
```

### 环境判断
```js
//__wxConfig这个内置对象在小程序官方文档没查到，所以慎用，自测可以
let version = __wxConfig.envVersion;
if (!version) version = __wxConfig.platform;
// console.log('版本号', version)
switch (version) {
    case 'devtools': //开发版
    return 'https://xxx.com';
    break;
    case 'develop': //真机调试版
    return 'https://xxx.com';
    break;
    case 'trial': //体验版
    return 'https://xxx.com';
    break;
    case 'release': //正式版
    return 'https://bbb.com';
    break;
    default:
    return 'https://bbb.com';
}
```

### 自定义tabbar
和自定义组件相同
特别注意要在自定义组件中定义：
```js
/**
 * 组件的对外属性，是属性名到属性设置的映射表
 */
properties: {
    //在这里初始化定义对外的数据
  selected: Number
},
// 在tabbar页面要调用getTabBar()方法，来高亮选择，以兼容某些安卓手机
onLoad(){
    if (typeof this.getTabBar === 'function' &&
      this.getTabBar()) {
      this.getTabBar().setData({
        selected: 0
      })
    }
}
```
`env(safe-area-inset-bottom)`这个css属性只适用于苹果手机，该变量是IOS 系统内核提供的，在IOS上正常使用；而安卓和开发工具上用的是 Chromium 内核，没有这个变量，所以不支持
官方文档给的tabbar示例中有个css方法：
padding-bottom: env(safe-area-inset-bottom);
这个方法是用来兼容全面屏手机，如iPhoneX，XR,XS等，会在底部留下安全距离，但是引发另一个问题，就是滚动时页面会暴露在tabbar下面，像这样：
![](http://www.guoxh.com/blog/img/blog/min-p.png)
其实如果不是这种凸出来的，加个`background`就搞定了，相当于挡住了下面的页面，而像上面这种就不能简简单单`background`了,
这时候可以取个巧，利用背景渐变来达到效果：
```css
.tab-bar{
    padding-bottom: env(safe-area-inset-bottom);
    background-image: linear-gradient(transparent 35%,#fff 35%)
}
```

* !!!不要把自定义tabbar文件夹放在最外层，否则会导致全局引用

### swiper指示点样式控制
```css
/* 注意要加重点标记 */
.wx-swiper-dot {
  display: inline-block !important;
  width: 6rpx !important;
  height: 6rpx !important;
  margin-right: 8rpx !important;
}

.wx-swiper-dot-active {
  width: 12rpx !important;
  height: 6rpx !important;
  border-radius: 3rpx !important;
}
.wx-swiper-dot::before{
    content: '';
    flex-grow: 1; 
    background: rgba(255,255,255,0.8);
    border-radius: 8rpx
}
.wx-swiper-dot-active::before{
    background:rgba(244,0,0,0.8);   
}
```
swiper-item一张时，真机调试出现不显示的情况，暂时处理为一张时不用swiper
找到解决方法了，哈哈哈哈哈哈哈，e，
每次更新数据的时候(数组的长度可能减小),所以记得:
```html
<swiper current="{{current}}">
```
```js
this.setData({
    current:0 // current的值不能大于list.length,所以每次更新数据的时候重置为0
})
```

### 小程序中ios不支持时间格式为 - 或者 . 的转换必须转换为 / 格式
例如：new Date("2019-07-05 12:00:00".replace(/-/g, "/"));
只有这样可以得到正确的时间格式，其他转换可能也行，但是没试

### 去除button默认样式
```css
button::after{
  border:none;
}
button{
  outline:none;
  border:none;
  list-style: none;
  padding:0;
  line-height: normal;
  margin: 0rpx;
  background-color: #fff;
  border-radius: 0rpx;
}
```

### 分享图片华为手机不支持本地图片，会有50%概率读取不到，亲测，放在cdn上是可以的

### js canvas生成二维码，华为手机显示不出来，亲测，调用两边draw方法就可以成功

### wx.chooseImage中使用wx.showLoading

* wx.chooseImage方法中使用wx.showLoading会导致wx.showLoading不显示，微信客户端bug，初步方案为加个setTimeout方法可以解决，时间必须设置300ms以上

### 自定义组件

#### 自定义组件中css不建议使用标签选择器，手机调试会看到警告提醒，page中可以使用

#### 自定义组件中获取canvas2d元素与page中获取方法不同：
pages中：
```js
wx.createSelectorQuery().select('#canvas').fields({
    node: true,
    size: true,
}).exec((res) => {
    const canvas = res[0].node;
    const ctx = canvas.getContext('2d');
})
```
自定义组件中：
```js
wx.createSelectorQuery().in(this).select('#canvas').fields({
    node: true,
    size: true,
}).exec((res) => {
    const canvas = res[0].node;
    const ctx = canvas.getContext('2d');
})
```
* 区别在于组件中要先调用`.in(this)`方法获取当前实例，才能再选择canvas

#### 自定义组件监听数据变化
属性值的改变情况可以使用 observer 来监听。目前，在新版本基础库中不推荐使用这个字段，而是使用 Component 构造器的 observers 字段代替，它更加强大且性能更好。
```js
Component({
  properties: {
    min: {
      type: Number,
      value: 0,
      observer: function(newVal, oldVal) {
        // 属性值变化时执行 不推荐
      }
    }
  },
  observers: {
    //推荐这种方式监听
    "data": function (data) {
      this.setData({
        allData: data
      })
      this.formateData(this.data.allData)
    }
  },
})
```

### scroll-view 组件子元素高度不够无法刷新解决办法
[传送门](https://developers.weixin.qq.com/community/develop/article/doc/000a806cc2c4b0055c0a6a7735b013)
```html
<scroll-view  ...>
  <block wx:for="{{dataList}}" wx:key="index">
    <view>{{item.id}}</view>
  </block>
  <view style="width:2rpx;height:2rpx;bottom:-2rpx;position:absolute;" />
</scroll-view>
```
```html
<scroll-view ...>
  <view style="width:100%;height:100%;padding-bottom: 20vw;">
  </view>
</scroll-view>
```
原理都相同，加个空`view标签`撑起内容高度，我试了第二种实现了，第一种没实现

svroll-view下拉刷新有时会莫名抖动2020.07.24

### 用npm包时，先检查根目录是否有package.json文件，没有新建一个，再npm install

### list页跳转detail页，detail页操作数据后，list做到不刷新同步数据
非常简单，利用小程序中自带方法`getCurrentPages()`可以获取上个页面数据方法的特性：
```js
// 在detail调用列表页方法，以实现不刷新页面改变状态
let pages = getCurrentPages();
if (pages.length >= 2 && pages[pages.length - 2].route == "page/list/list") {
  //如果上个页面为list页
  let preData = pages[pages.length - 2];
  let arg = {
    resultFlag: res.infoMap.resultFlag,
    id: item.id,
    type: type,
    count: res.infoMap.count
  };
  //调用list方法更新数据
  preData.detailSetData(arg)
}
```

### wx.setNavigationBarTitle方法不区分页面，导致异步请求未完成返回会显示在前一个页面
```js
//处理方法就是在异步请求之前和之后分别记录当前页面路由，如果一致才设置tabbarText
let page = getCurrentPages();
let currentRoute = page[page.length - 1].route;
setTimeout(()=>{
  //定时器模拟异步请求
  let page2 = getCurrentPages();
  let currentRoute2 = page2[page2.length - 1].route;
  if (currentRoute == currentRoute2) {
    wx.setNavigationBarTitle({
      title: res.result.goods_name
    })
  }
},1000)
//哪有人要说了，直接在前一个页面重新set一下不就好了，但是你要知道我们无法知道异步请求成功时间，所以不可靠
```

### 模板语法

1.定义模板，使用`name`属性作为模板名称，然后再`template`标签中写入代码，或者引入自定义组件
```wxml
<template name="msgItem">
  <view>
    <text> {{index}}: {{msg}} </text>
    <text> Time: {{time}} </text>
  </view>
</template>
```

2.使用模板，使用`is`属性，声明要使用的模板，然后将上面定义的模板中需要的数据引入：
```wxml
<template is="msgItem" data="{{...item}}"/>
```
```js
Page({
  data: {
    item: {
      index: 0,
      msg: 'this is a template',
      time: '2016-09-15'
    }
  }
})
```
> `is`属性还可以使用Mustache胡子语法，动态引入需要的模板

### 小程序插件的大小是会算进小程序代码包2M体积限制中的，所以如果遇到插件包过大，可以考虑引用较低版本的插件，当然得考虑兼容性；
