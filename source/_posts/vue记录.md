---
title: vue记录
date: 2017-02-22 11:34:59
tags: vue
categories: vue
---

### 如何让css只在当前组件起作用
这样写其他组件引用当前组件时当前组件中的css无法作用在其他组件（个人认为中小项目不需要加这个属性，因为组件之间的调用会很少很少）
同级组件不加scope如果有相同类名，会相互影响（同级当前组件优先级高）
```
<style scoped></style>
```

### v-for遍历数据中的v-bind:key（:key）问题
> ***tips:*** 2.2.0+ 的版本里，当在组件中使用 v-for 时，key 现在是必须的。
> 在vue中用v-for最好搭配v-bind:key="key"来使用；
> key的作用主要是为了高效的更新虚拟DOM
> key值只能为string/number，不能是对象或数组；
> example: <el-carousel-item v-for="(item, index) in items" :key="index"></el-carousel-item>
我们知道，vue和react都实现了一套虚拟DOM，使我们可以不直接操作DOM元素，只操作数据便可以重新渲染页面。而隐藏在背后的原理便是其高效的Diff算法。

### 父组件与子组件互传数据
1. props:{}用来接收父组件传过来的数据
* 父组件：
![](http://ota5i8p1g.bkt.clouddn.com/parentComponets2.png)
![](http://ota5i8p1g.bkt.clouddn.com/parentComponent1.png)
* 子组件：
![](http://ota5i8p1g.bkt.clouddn.com/childComponets.png)
![](http://ota5i8p1g.bkt.clouddn.com/childCpmponents2.png)

2. this.$emit(自定义事件名，数据)向父组件传递数据
* 子组件：
![](http://ota5i8p1g.bkt.clouddn.com/child1.png)
![](http://ota5i8p1g.bkt.clouddn.com/child2.png)

* 父组件
![](http://ota5i8p1g.bkt.clouddn.com/parent1.png)
![](http://ota5i8p1g.bkt.clouddn.com/parent2.png)

`1this.$emit(自定义事件,[])用来触发自定义事件increment1(或者函数名吧)，[]为参数`

### this.$set()设置对象的属性，这个方法主要用于避开 Vue 不能检测属性被添加的限制
```
var vm = new Vue({
el:'#app',
data:{
  arr:[1,2,3];
}
})
vm.arr[0] = 4;//这样操作数据，dom不会更新数据
vm.$set(vm.arr,0,4) //这样dom数据才会改变
```

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

### 在所有ajax完成后执行
这个时候用promise.all最合适了，比如：
```
let p1=new Promise(function(resolve,reject){ 
resolve(42)
});
let p2=new Promise(function(resolve,reject){ 
resolve(43)
});
let p3=new Promise(function(resolve,reject){ 
resolve(44)
});
let p4=Promise.all([p1,p2,p3]);
p4.then(function(v){
这个里面就是你需要做的事情
})
```

### 深入理解vue
当你把一个普通的 JavaScript 对象传给 Vue 实例的 data 选项，Vue 将遍历此对象所有的属性，并使用 Object.defineProperty 把这些属性全部转为 getter/setter。
每个组件实例都有相应的 watcher 实例对象，它会在组件渲染的过程中把属性记录为依赖，之后当依赖项的 setter 被调用时，会通知 watcher 重新计算，从而致使它关联的组件得以更新。

### 组件
单文件组件：你完全可以自定义一个组件名，然后在父级组件中import组件，并且在components: {}中声明一下

### 在v-html中使用filters 的三种方法
1.在vue实例上定义全局方法
```
Vue.prototype.highlight= function (sTitle) {
  // to do
};
然后所有组件都可以使用方法
v-html="highlight(option.title)"
```
2.使用 $options.filters
```
v-html="$options.filters.highlight(option.title)"
var appMain= new Vue({
    el: '#appMain',
    filters:{
      highlight: function(msg) {
          return msg.replace(/\n/g, "<br>") ;
      }
    },
    data: {
     return{}
   }
})
```
3.computed 计算属性
```
var appMain= new Vue({
      el: '#appMain',
      computed :{
         content： function (msg) {
			return msg.replace("\n", "<br>")
          },
      },
      data: {
        content: "XXXXX"
     }
}）
页面上：
<div>{{content}}</div>
```

### 文件上传（支持PDF格式文件上传）以文件流的形式上传到接口
```
<input type="file" id="CarDamageFile" class="g-core-image-upload-form" value="" accept="image/gif,image/jpeg,image/jpg,image/png,application/pdf" data-type="back-page" name="Pictures" multiple="multiple" @change="imagechanged($event,Udata[7])" />
//文件上传
imagechanged(e, data) {
//参数e为当前上传的表单，data为要处理的数据
    var that = this,
      _data = that.Udata,
      fileList = e.target.files;
    data.count = 0;
    if (fileList.length > 5) {
      that.wrongTip("每项最多上传5张");
      return;
    }
    if (fileList) {
      for (var i = 0; i < fileList.length; i++) {
		//遍历上传的文件，一个一个处理
        that.readFile(fileList.length, fileList[i], data);
      }
    }
  },
  readFile(num, res, Sdata) {
//num为上传文件数量，res为上传文件数据，Sdata为要处理数据
    var that = this,
      _data = that.Udata,
      Ename = Sdata.Ename,
      render = new FileReader();
    if (res.type != "application/pdf" && res.size / 1024 > 10240) {
      that.wrongTip(
        "索赔材料仅限pdf. jpg .gif .png格式，单个图片大小不超过10M，单个PDF大小不超过4.5M，每项最多上传5张。"
      );
      return;
    } else if (res.type == "application/pdf" && res.size / 1024 > 4608) {
      that.wrongTip(
        "索赔材料仅限pdf. jpg .gif .png格式，单个图片大小不超过10M，单个PDF大小不超过4.5M，每项最多上传5张。"
      );
      return;
    } else {
      render.addEventListener(
        "load",
        function() {
          var src = this.result;
          var formData = new FormData();
          var index = this.index;
          var _fileName = res.name;
          //convertBase64UrlToBlob函数是将base64编码转换为Blob
          formData.append("fileUrl", res);
          formData.append("fileName", _fileName);
          Sdata.loading = true;
          Basic.ajaxReturnPromise({
            type: "POST",
            apiUrl: "/Comment/UploadImg",
            customLoading: true,
            param: formData,
            emulateJSON: false
          }).then(
            response => {
              if (response.IsSuccess) {
                ++Sdata.count;
                var data = response.data;
                if (Sdata.imgSrc.length + data.length < 6) {
                  Sdata.curImg = parseInt(Sdata.curImg + 1);
                  // Sdata.imgSrc.shift();
                  for (var i in data) {
                    if (data[i].BaseUrl.toLowerCase().indexOf(".pdf") != -1) {
                      Sdata.imgSrc.unshift({
                        picUrl: "https://h5-fe.huizuche.com/claim/PDF.png",
                        url: data[i].BaseUrl,
                        name: _fileName
                      });
                    } else {
                      Sdata.imgSrc.unshift({
                        picUrl:
                          data[i].BaseUrl +
                          "?x-oss-process=image/resize,m_fill,h_78,w_78",
                        url: data[i].BaseUrl,
                        name: _fileName
                      });
                    }
                  }
                  that.$store.dispatch(Sdata.store, {
                    [Ename]: Sdata.imgSrc
                  });
                } else {
                  that.wrongTip("每项最多上传5张");
                }
              }
              if (Sdata.count == num) {
                Sdata.loading = false;
              }
            },
            err => {
              that.wrongTip("文件上传失败，请重新上传");
            }
          );
        },
        false
      );
      render.readAsDataURL(res);
    }
  }
```

### vue微信分享
```javascript
1.页面中引入微信分享JS文件
<script src="https://res.wx.qq.com/open/js/jweixin-1.0.0.js"></script>

2.//微信分享方法相关配置
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
        Vue.http.get('/Api/Activity/GetWeixinAPIConfig?url=' + href).then(response => {
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
