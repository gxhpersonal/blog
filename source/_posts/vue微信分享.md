---
title: vue微信分享
date: 2019-03-26 11:54:07
tags: vue
categories: vue
---

### vue微信分享

1.页面中引入微信分享JS文件
```html
<script src="http://res.wx.qq.com/open/js/jweixin-1.4.0.js"></script>
```
> 原有的 wx.onMenuShareTimeline、wx.onMenuShareAppMessage、wx.onMenuShareQQ、wx.onMenuShareQZone 接口，即将废弃。
请尽快迁移使用客户端6.7.2及JSSDK 1.4.0以上版本支持的 wx.updateAppMessageShareData、updateTimelineShareData 接口。

2.微信分享方法相关配置
```javascript
import Vue from 'vue';
import vueResource from 'vue-resource';
export default {

    setWxConfig: function () {
		//判断环境，因为二次分享Android和ios有不同的bug//测试发现只有设置了路由会有问题，#/在微信中会被截断，导致分享链接和传入API的链接不一样
		var href;
		if (navigator.userAgent.match(/(iPhone|iPod|iPad);?/i)) {
            href = encodeURIComponent(window.location.href);
        } else if (navigator.userAgent.match(/android/i)) {
            href = window.location.href;
        } else {
            href = window.location.href;
        }
        Vue.http.options.emulateJSON = true;
        //下面请求接口来获取所需参数
        Vue.http.get('/Api/Activity/GetWeixinAPIConfig?url=' + href).then(response => {
            let weixinConfigData = response.body;
            wx.config({
                debug: false, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
                appId: weixinConfigData.AppId,// 必填，公众号的唯一标识
                timestamp: weixinConfigData.Timestamp,// 必填，生成签名的时间戳
                nonceStr: weixinConfigData.NonceStr,// 必填，生成签名的随机串
                signature: weixinConfigData.signature,// 必填，签名
                jsApiList: [
                    'checkJsApi',
                    'updateAppMessageShareData',
                    'updateTimelineShareData',
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
                ]// 必填，需要使用的JS接口列表
            });
        }, response => {
            // error callback
        });
    },
    wxShare(obj) {
        wx.ready(function () { ////需在用户可能点击分享按钮前就先调用
			      // “分享给朋友”及“分享到QQ”
            wx.updateAppMessageShareData({
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
			    // “分享到朋友圈”及“分享到QQ空间”
            wx.updateTimelineShareData({
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
> 附录：[获取权限签名的算法](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421141115)
