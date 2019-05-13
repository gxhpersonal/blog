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
    }
})
```
> find() 方法返回通过测试（函数内判断）的数组的第一个元素的值。
find() 方法为数组中的每个元素都调用一次函数执行：
当数组中的元素在测试条件时返回 true 时, find() 返回符合条件的元素，之后的值不会再调用执行函数。
如果没有符合条件的元素返回 undefined
注意: find() 对于空数组，函数是不会执行的。
注意: find() 并没有改变数组的原始值。