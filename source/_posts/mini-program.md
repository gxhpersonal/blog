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
```
* 不要把自定义tabbar文件夹放在最外层，否则会导致全局引用