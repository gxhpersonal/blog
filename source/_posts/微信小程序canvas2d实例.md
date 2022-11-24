---
title: 微信小程序canvas2d实例
date: 2020-04-24 09:49:16
tags: 微信小程序
categories: 微信小程序
---

### canvas2d生成图片，本人菜，所以踩了很多坑，记录下步骤代码，温故而知新
```js
//保存图片
saveImg() {
  //获取canvas实例，因为要获取canvas节点，所以不能使用wx:if隐藏
  const query = wx.createSelectorQuery();
  query.select('#canvas')
    .fields({
      node: true,
      size: true
    })
    .exec(async (res) => {
      //要在获取node节点回调执行保存
      wx.canvasToTempFilePath({
        x: 0,
        y: 0,
        width: res[0].width * wx.getSystemInfoSync().pixelRatio,
        height: res[0].height * wx.getSystemInfoSync().pixelRatio,
        destWidth: res[0].width * wx.getSystemInfoSync().pixelRatio * 2,
        destHeight: res[0].height * wx.getSystemInfoSync().pixelRatio * 2,
        canvas: res[0].node,
        success(res) {
          console.log(res)
          wx.saveImageToPhotosAlbum({
            filePath: res.tempFilePath,
            success(res) {
              wx.showToast({
                title: '保存成功',
                icon: "none"
              })
              console.log(res)
            },
            complete(res) {
              console.log(res)
            }
          })
        },
        complete(res) {
          console.log(res)
        }
      })
    })
},
```
```js
//生成海报
getPost(e) {
  //因为要获取手机号生成邀请码所以要先判断登录
  if (wx.getStorageSync('isLogin')) {
    if (e.detail.errMsg != "getUserInfo:ok") {
      wx.showModal({
        title: '提示!',
        content: '请允许授权,否则无法生成海报!',
        showCancel: false
      })
      return false
    }
    let avatar = e.detail.userInfo.avatarUrl
    let nickname = e.detail.userInfo.nickName
    const that = this;
    wx.showLoading({
      title: '海报生成中...',
      mask: true
    })
    wx.createSelectorQuery().select('#canvas').fields({
      node: true,
      size: true,
    }).exec(async (res) => {
      //要在获取node节点之后回调执行绘画
      console.log(res)
      const canvas = res[0].node;
      const ctx = canvas.getContext('2d');
      const width = res[0].width;
      const height = res[0].height;
      if(this.data.flag){
        //这里加个开关只有第一次绘制才设置canvas宽高，因为多次点击显示隐藏不会重绘，所以会导致canvas画布越来越大
        const dpr = wx.getSystemInfoSync().pixelRatio
        canvas.width = width * dpr;
        canvas.height = height * dpr;
        ctx.scale(dpr, dpr);
        this.data.flag = false;
      }
      const bgImg = canvas.createImage();
      bgImg.src = wx.getStorageSync('postBg');
      let bgImgPo = await new Promise((resolve, reject) => {
        bgImg.onload = () => {
          resolve(bgImg)
        }
        bgImg.onerror = (e) => {
          reject(e)
        }
      });
      //背景图必须这样用await异步，否则背景图会把背景图上的文字遮盖，待老夫研究原理再补充
      ctx.drawImage(bgImgPo, 0, 0, 257, 389)
      //用户昵称
      ctx.font = "normal bold 14px sans-serif";
      ctx.fillStyle = "#FFFFFF"
      ctx.fillText(`${nickname}邀您参加`, 42, 24)
      //参与
      ctx.font = "normal bold 14px sans-serif";
      ctx.fillStyle = "#FFFFFF"
      ctx.strokeStyle = "#4B32A9"
      ctx.strokeText('长按识别二维码参与', 75, 373)
      ctx.fillText('长按识别二维码参与', 75, 373)

      //二维码裁剪
      function canvasQR(qrImgUrl) {
        const qrImg = canvas.createImage();
        qrImg.src = qrImgUrl;
        qrImg.onload = () => {
          ctx.save();
          ctx.beginPath() //开始创建一个路径
          ctx.arc(128, 319, 32, 0, 2 * Math.PI, false) //画一个圆形裁剪区域
          ctx.clip() //裁剪
          ctx.drawImage(qrImg, 96, 287, 65, 65);
          ctx.closePath();
          ctx.restore();
        }
      }
      canvasQR('http://www.guoxh.com/blog/img/blog/qr.png');

      //头像裁剪
      function canvasWxHeader(headImageLocal) {
        const headerImg = canvas.createImage();
        headerImg.src = headImageLocal;
        headerImg.onload = () => {
          ctx.save();
          ctx.beginPath() //开始创建一个路径
          ctx.arc(26, 21, 12, 0, 2 * Math.PI, false) //画一个圆形裁剪区域
          ctx.clip() //裁剪
          ctx.drawImage(headerImg, 14, 5, 24, 30);
          ctx.closePath();
          ctx.restore();
          //关闭loading
          wx.hideLoading();
        }
      }
      canvasWxHeader(avatar);

      that.setData({
        postPopup: true
      })
    })
  } else {
    wx.navigateTo({
      url: '/pages/login/login',
    })
  }

},
```

### canvas不能wx:if隐藏，否则会获取不到元素，可以使用fixed+top:-9999rpx实现

### canvas 2d生成背景图片不遮挡文字：使用await
```js
const bgImg = canvas.createImage();
      bgImg.src = 'http://www.guoxh.com/blog/img/blog/qr.png';
      let bgImgPo = await new Promise((resolve, reject) => {
        bgImg.onload = () => {
          resolve(bgImg)
        }
        bgImg.onerror = (e) => {
          reject(e)
        }
      });
      ctx.drawImage(bgImgPo, 0, 0, 257, 389)
```

## 设置ctx.font = "normal bold 14px sans-serif";一定要写全，或者不要写微信没有的字体，虽然我不知道微信没有什么字体，所以我就抄网上的，一定要记住，不然安卓会闪退/(ㄒoㄒ)/~~