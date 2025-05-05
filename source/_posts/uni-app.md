---
title: uni-app
date: 2023-10-17 14:52:19
tags: uni-app
categories: uni-app
---

### 条件编译快捷命令
选中要条件编译的代码块，`ctrl + alt + /`即可生成正确注释
```js
// #ifdef APP
uni.hideNavigationBarLoading();
// #endif
```

### 滚动一屏，sticky失效
需要把 sticky元素放在 一个 view 中，不能放在 template 中  

### app端选择地图导航
```js
toMapAPP(data) {
  const {
    lat,
    lng,
    shopName,
    name
  } = data;
  let url = "";
  if (plus.os.name == "Android") {
    //判断是安卓端
    plus.nativeUI.actionSheet({
        //选择菜单
        title: "选择地图应用",
        cancel: "取消",
        buttons: [{
            title: "百度地图",
          },
          {
            title: "高德地图",
          },
        ],
      },
      function(e) {
        switch (e.index) {
          //下面是拼接url,不同系统以及不同地图都有不同的拼接字段
          case 1:
            url =
              `baidumap://map/marker?location=${lat},${lng}&title=${name||shopName}&src=taoliangche&coord_type=gcj02`
            break;
          case 2:
            url =
              `androidamap://arroundpoi?sourceApplication=softname&keywords=${name||shopName}&lat=${lat}&lon=${lng}&dev=0`;
            break;
          default:
            break;
        }
        if (url != "") {
          url = encodeURI(url);
          plus.runtime.openURL(url, function(e) {
            plus.nativeUI.alert("本机未安装指定的地图应用");
          });
        }
      }
    );
  } else {
    // iOS上获取本机是否安装了百度高德地图，需要在manifest里配置
    // 在manifest.json文件app-plus->distribute->apple->urlschemewhitelist节点下添加
    //（如urlschemewhitelist:["iosamap","baidumap"]）
    plus.nativeUI.actionSheet({
        title: "选择地图应用",
        cancel: "取消",
        buttons: [
          // 	{
          // 	title: "腾讯地图"
          // },
          {
            title: "百度地图",
          },
          {
            title: "高德地图",
          },
        ],
      },
      function(e) {
        switch (e.index) {
          // case 1:
          // 	url = `qqmap://map/geocoder?coord=${lat},${lng}&referer=xxx`;
          // 	break;
          case 1:
            url =
              `baidumap://map/marker?location=${lat},${lng}&title=${shopName||name}&src=taoliangche&coord_type=gcj02`;
            break;
          case 2:
            url =
              `androidamap://arroundpoi?sourceApplication=softname&keywords=${shopName||name}&lat=${lat}&lon=${lng}&dev=0`;
            break;
          default:
            break;
        }
        if (url != "") {
          url = encodeURI(url);
          plus.runtime.openURL(url, function(e) {
            plus.nativeUI.alert("本机未安装指定的地图应用");
          });
        }
      }
    );
  }
},
```

### app分享微信小程序
```js
// 分享到微信小程序
share() {
  // #ifdef APP
  uni.share({
    provider: "weixin",
    scene: "WXSceneSession",
    type: 5,
    miniProgram: {
      id: '原始小程序 id',
      path: '/pages/home/home?id=' + this.cid + '&uid=' + uni.getStorageSync('uid'),
      type: 0,
      webUrl: "www.123.com"
    },
    title: this.info.name,
    summary: "描述",
    imageUrl: "share.png",
    success: function(res) {
      console.log("success:" + JSON.stringify(res));
    },
    fail: function(err) {
      console.log("fail:" + JSON.stringify(err));
    }
  });
  // #endif
}
```

注意事项：
1. 报错：应用与小程序未在同一个开放平台下：因为直接运行到标准基座了，这个时候可以打自定义基座
2. 报错：share:fail [Share微信分享:-3]Unable to send, https://ask.dcloud.net.cn/article/287
原因一 miniProgram中的webUrl 未填写，随便填写一个地址就行了
原因二 uni.share 中的imageUrl 图片大小过大，应该小于20kb
3. 报错：不支持的分享类型
检查 uni.share 中miniProgram参数中的id要填写小程序原始id，不是小程序的appid，原始 id在小程序后台-账号设置-账号信息-原始ID
