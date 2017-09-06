---
title: vue常见问题
date: 2017-02-22 11:34:59
tags: vue
categories: vue
---

### 如何让css只在当前组件起作用
在style标签中加入scoped语句
```
<style scoped></style>
```

css支持scss语法：
```
<style lang="scss"></style>
```

### v-for遍历数据中的v-bind:key（:key）问题
> ***tips:*** 2.2.0+ 的版本里，当在组件中使用 v-for 时，key 现在是必须的。
> 在vue中用v-for最好搭配v-bind:key="key"来使用；
> key的作用主要是为了高效的更新虚拟DOM
我们知道，vue和react都实现了一套虚拟DOM，使我们可以不直接操作DOM元素，只操作数据便可以重新渲染页面。而隐藏在背后的原理便是其高效的Diff算法。

### props:{}用来接收父组件传过来的数据

### 引入模块和引入文件
*引入模块：import webview from "../../common/webview.js";
*引入文件：import "../../filter/webpFilter.js";

### vue数据绑定支持JS表达式(以后v-for的时候再也不用先在JS中处理一遍数据再绑定了)
```
{{ number + 1 }}
{{ ok ? 'YES' : 'NO' }}
{{ message.split('').reverse().join('') }}
<div v-bind:id="'list-' + id"></div>
```

### vue中的index索引值问题
vue 中$index $key 已经移除了
之前可以这样:
```
<ul id="example">
    <li v-for="item in items">
        {{$index}}
        {{$key}}
    </li>
</ul>
```
现在已经移除,如果还用的话就会报错:Uncaught ReferenceError: $index is not defined;

现在这样写:
```
<ul id="example">
    <li v-for="(item,index) in items">
        {{item}}
        {{index}}
    </li>
</ul>
```
第一个参数是值,第二个参数是索引;目的是为了保持和原生的一致;

### 鼠标右键事件
@contextmenu.prevent="alert('您点击的是鼠标右键')"

### vue微信分享
```javascript
1.页面中引入微信文件
<script src="https://res.wx.qq.com/open/js/jweixin-1.0.0.js"></script>

2.//微信分享方法相关配置
import Vue from 'vue';
import vueResource from 'vue-resource';
export default {

    setWxConfig: function () {
        Vue.http.options.emulateJSON = true;
        Vue.http.get('/Api/Activity/GetWeixinAPIConfig?url=' + window.location.href).then(response => {
            let weixinConfigData = response.body;
            wx.config({
                debug: false,
                appId: weixinConfigData.AppId,
                timestamp: weixinConfigData.Timestamp,
                nonceStr: weixinConfigData.NonceStr,
                signature: weixinConfigData.signature,
                jsApiList: [
                    'checkJsApi',
                    'onMenuShareTimeline',
                    'onMenuShareAppMessage',
                    'onMenuShareQQ',
                    'onMenuShareWeibo',
                    'hideMenuItems',
                    'showMenuItems',
                    'hideAllNonBaseMenuItem',
                    'showAllNonBaseMenuItem',
                    'translateVoice',
                    'startRecord',
                    'stopRecord',
                    'onRecordEnd',
                    'playVoice',
                    'pauseVoice',
                    'stopVoice',
                    'uploadVoice',
                    'downloadVoice',
                    'chooseImage',
                    'previewImage',
                    'uploadImage',
                    'downloadImage',
                    'getNetworkType',
                    'openLocation',
                    'getLocation',
                    'hideOptionMenu',
                    'showOptionMenu',
                    'closeWindow',
                    'scanQRCode',
                    'chooseWXPay',
                    'openProductSpecificView',
                    'addCard',
                    'chooseCard',
                    'openCard'
                ]
            });
        }, response => {
            // error callback
        });
    },
    wxShare(obj) {
        wx.ready(function () {

            wx.onMenuShareAppMessage({
                title: obj.shareTitle,
                desc: obj.descContent,
                link: obj.lineLink,
                imgUrl: obj.imgUrl,
                trigger: function (res) {
                    //alert('用户点击发送给朋友');
                },
                success: function (res) {
                    // alert('已分享');
                },
                cancel: function (res) {
                    //alert('已取消');
                },
                fail: function (res) {
                    //  alert(JSON.stringify(res));
                }
            });

            wx.onMenuShareTimeline({
                title: obj.shareTitle,
                desc: obj.descContent,
                link: obj.lineLink,
                imgUrl: obj.imgUrl,
                trigger: function (res) {
                    // alert('用户点击分享到朋友圈');
                },
                success: function (res) {
                    // alert('已分享');
                },
                cancel: function (res) {
                    // alert('已取消');
                },
                fail: function (res) {
                    // alert(JSON.stringify(res));
                }
            });

            wx.onMenuShareQQ({
                title: obj.shareTitle,
                desc: obj.descContent,
                link: obj.lineLink,
                imgUrl: obj.imgUrl,
                success: function () {
                    // 用户确认分享后执行的回调函数
                },
                cancel: function () {
                    // 用户取消分享后执行的回调函数
                }
            });
        });
    }
}

3.在指定组件中引入公共微信分享方法文件；就可以调用分享方法
import wechat from "../../common/wechat.js";
wechat.setWxConfig();
wechat.wxShare({ 
shareTitle: "这是微信分享的标题", 
descContent: "这是微信分享的描述文案", 
lineLink: location.href, 
imgUrl: "这是微信分享的图片" });
```
