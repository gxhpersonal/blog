---
title: miniProgram
date: 2019-03-05 14:50:11
tags: miniProgram
categories: miniProgram
---

### 禁止下拉
app.json中window下加enablePullDownRefresh:false

### 订单列表倒计时
```js
//添加倒计时参数res为接口返回数据列表
res.find((v) => {
    let nowTime = new Date().getTime();
    //expire_time为接口返回截止时间的时间戳
    let endDate = v.expire_time * 1000 - nowTime;
    if (endDate > 0) {
    //倒计时
    let stop = setInterval(() => {
        //days | hour 分别表示天和小时
        //days = Math.floor(time.t / 1000 / 60 / 60 / 24);
        //hour = Math.floor(time.t / 1000 / 60 / 60 % 24);
        let minute = Math.floor((endDate / 1000 / 60) % 60);
        let second = Math.floor((endDate / 1000) % 60);
        let min = minute < 10 ? "0" + minute : minute;
        let sec = second < 10 ? "0" + second : second;
        if (endDate <= 0) {
        //如果倒计时结束，改变当前订单状态，重新赋值orderList数据即可
        v.status = -1;
        this.setData({
            orderList: this.data.orderList
        })
        clearInterval(stop);
        return false;
        } else {
        endDate -= 1000;
        }
        v.goods_time = min + ":" + sec;
        //每秒重新赋值orderList数据
        this.setData({
        orderList: this.data.orderList
        })
    }, 1000);
    } else {
       v.status = -1;
       this.setData({
         orderList: this.data.orderList
    })
    }
})
```
> find() 方法返回通过测试（函数内判断）的数组的第一个元素的值。
find() 方法为数组中的每个元素都调用一次函数执行：
当数组中的元素在测试条件时返回 true 时, find() 返回符合条件的元素，之后的值不会再调用执行函数。
如果没有符合条件的元素返回 undefined
注意: find() 对于空数组，函数是不会执行的。
注意: find() 并没有改变数组的原始值。


### 摇一摇实现相应操作
```js
onShow: function() {
    //重力加速度
    wx.onAccelerometerChange(function (res) {
        //console.log(res.x)
        //console.log(res.y)
        // console.log(res.z)
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

### web-view正确使用
小程序页面：
```wxml
<navigator wx:for="{{imgUrlNew}}" wx:key="index" url="/pages/webview/webview?skipUrl={{item.url}}">
     <image src='{{item.img}}' mode="widthFix"></image>
     <text>{{item.name}}</text>
</navigator>
```
封装webview的页面：
```wxml
<!-- webview.wxml -->
<web-view src="{{skipUrl}}"></web-view>
```
然后在webview.js中：
```wxjs
data: {
id:'',
imgUrl:''
},
onLoad: function (options) {
    //获取到链接中的webview链接参数
    this.setData({
      skipUrl: options.skipUrl
    })
  },
```

### 环境判断
```js
//__wxConfig这个内置对象在小程序官方文档没查到，所以慎用，自测可以
let version = __wxConfig.envVersion;
if (!version) version = __wxConfig.platform;
// console.log('版本号', version)
switch (version) {
    case 'devtools': //开发版
    return 'https://svip-api-test.jia-expo.com';
    break;
    case 'develop': //真机调试版
    return 'https://svip-api-test.jia-expo.com';
    break;
    case 'trial': //体验版
    return 'https://svip-api-test.jia-expo.com';
    break;
    case 'release': //正式版
    return 'https://svip-api.51jiabo.com';
    break;
    default:
    return 'https://svip-api.51jiabo.com';
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

* 不要把自定义tabbar文件夹放在最外层，否则会导致全局引用

### swiper指示点样式控制
```css
.wx-swiper-dots{
  position: absolute;
  bottom: 50rpx;
}
.swiper-box .wx-swiper-dot::before{
    content: '';
    flex-grow: 1; 
    background: rgba(255,255,255,0.8);
    border-radius: 8rpx
}
.swiper-box .wx-swiper-dot-active::before{
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

### 父子自定义组件传参+时间绑定
子组件：
```html
<view class="close" bindtap='onClose'></view>
```
```js
/**
 * 组件的方法列表
 */
methods: {
    onClose(){
        //triggerEvent方法参数：事件名、detail对象和事件选项
        this.triggerEvent("closeReserve",{"name":"我是子组件传给父组件的数据"})
    }
}
```
父组件：
```html
<hot-goods-popup wx:if="{{reserveSuccess}}" bindcloseReserve="closeReserve" />
```
```js
/**
 * 组件的方法列表
 */
methods: {
    closeReserve(){
        this.setData({
        reserveSuccess: false
        })
    }
}
```


### 自定义组件中不建议使用标签选择器，手机调试会看到报错提醒，page中可以使用

### 小程序中不支持时间格式为 - 或者 . 的转换必须转换为 / 格式
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
}
```