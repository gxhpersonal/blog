---
title: vue微信分享
date: 2019-03-26 11:54:07
tags: vue
categories: vue
---

### vue微信分享

1.页面中引入微信分享JS文件 或者 执行`npm install weixin-js-sdk --save`命令安装js包
```html
<script src="http://res.wx.qq.com/open/js/jweixin-1.4.0.js"></script>
<!-- or -->
`npm install weixin-js-sdk --save`安装
```
> 原有的 wx.onMenuShareTimeline、wx.onMenuShareAppMessage、wx.onMenuShareQQ、wx.onMenuShareQZone 接口，即将废弃。
请尽快迁移使用客户端6.7.2及JSSDK 1.4.0以上版本支持的 wx.updateAppMessageShareData、updateTimelineShareData 接口。

2.微信分享方法相关配置，新建文件`wechat.js`
```javascript
// 微信分享
import wx from "weixin-js-sdk";
//请求封装见[vue-axios请求-封装](http://www.guoxh.com/blog/2019/03/26/vue-axios%E8%AF%B7%E6%B1%82-%E5%B0%81%E8%A3%85/)
import { request } from '../js/request';

const requestget = new request();
//判断环境，因为二次分享Android和ios有不同的bug//测试发现只有设置了路由会有问题，#/在微信中会被截断，导致分享链接和传入API的链接不一样
let href;
var ua = window.navigator.userAgent.toLowerCase();
if (ua.match(/MicroMessenger/i) == 'micromessenger') {
     if (navigator.userAgent.match(/(iPhone|iPod|iPad);?/i)) {
    href = encodeURIComponent(window.location.href);
} else if (navigator.userAgent.match(/android/i)) {
    href = window.location.href;
} else {
    href = window.location.href;
}		
requestget.Get({ url: '/getWxConfig', data: { "url": href } }).then((data) => {
    weixinConfigData = data.data;
    wx.config({
        debug: false, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
        appId: weixinConfigData.appid,// 必填，公众号的唯一标识
        timestamp: weixinConfigData.timestamp,// 必填，生成签名的时间戳
        nonceStr: weixinConfigData.noncestr,// 必填，生成签名的随机串
        signature: weixinConfigData.signature,// 必填，签名
        jsApiList: [
            'checkJsApi',
            'updateAppMessageShareData',
            'updateTimelineShareData',
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
}).catch((e) => {

})
}

class wechatShareApi {
    wxShare(param) {
        wx.ready(function () {
            // 分享给朋友及“分享到QQ
            wx.updateAppMessageShareData({
                desc: param.descContent,
                title: param.shareTitle,
                link: param.lineLink,
                imgUrl: param.imgUrl,
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
                    //alert(JSON.stringify(res));
                }
            });

            // 分享到朋友圈及“分享到QQ空间”
            wx.updateTimelineShareData({
                desc: param.descContent,
                title: param.friendTitle == undefined ? param.shareTitle : param.friendTitle,
                link: param.lineLink,
                imgUrl: param.imgUrl,
                trigger: function (res) {
                    //alert('用户点击发送给朋友');
                },
                success: function (res) {
                    //alert('已分享');

                },
                cancel: function (res) {
                    //alert('已取消');
                },
                fail: function (res) {
                    // alert(JSON.stringify(res));
                }
            });

        });
    }
}
export {
    wechatShareApi
}

3.在指定组件中引入公共微信分享方法文件`wechat.js`；就可以调用分享方法
import {wechatShareApi} from "../assets/js/wechat"
const weApi = new wechatShareApi();
//微信分享参数
weApi.wxShare({
    shareTitle: "这是微信分享的标题", 
    descContent: "这是微信分享的描述文案", 
    lineLink: location.href, 
    imgUrl: "这是微信分享的图片" 
})
```
> 附录：[获取权限签名的算法](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421141115)
