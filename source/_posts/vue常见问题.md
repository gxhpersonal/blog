---
title: vue常见问题
date: 2017-02-22 11:34:59
tags:
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

### v-for遍历数据问题
> 在vue中用v-for必须搭配v-bind:key="key"来使用；

### props:{}用来接收父组件传过来的数据

### 引入模块和引入文件
*引入模块：import webview from "../../common/webview.js";
*引入文件：import "../../filter/webpFilter.js";


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
shareTitle: "惠租车-驾照全球通国家查询", 
descContent: "[驾照全球通]全球自驾200国，顶级车行认证！", 
lineLink: location.href, 
imgUrl: "https://cdn-qiniu-app1.huizuche.com/2.6/logo_share.jpg" });
```
