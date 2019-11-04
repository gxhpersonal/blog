---
title: vue记录
date: 2017-02-22 11:34:59
tags: vue
categories: vue
---

### 如何让css只在当前组件起作用
这样写其他组件引用当前组件时当前组件中的css无法作用在其他组件（个人认为中小项目不需要加这个属性，因为组件之间的调用会很少很少）
同级组件不加scope如果有相同类名，会相互影响（同级当前组件优先级高）
```css
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
![](http://www.guoxh.com/blog/img/vue/propParentDOM.png)
* 子组件：
![](http://www.guoxh.com/blog/img/vue/propChildDOM.png)

2. this.$emit(自定义事件名，数据)向父组件传递数据
* 子组件：
![](http://www.guoxh.com/blog/img/vue/$emitChildDOM.png)
![](http://www.guoxh.com/blog/img/vue/$emitChildJS.png)

* 父组件
![](http://www.guoxh.com/blog/img/vue/$emitParentDOM.png)
![](http://www.guoxh.com/blog/img/vue/$emitParentJS.png)
或者父组件中：
```js
data(){
	return{
		a:'',
		b:''
	}
},
mounted(){
	//两种接收的方式    
	var _this = this;
	Event.$on('transferUser',function(a){
		_this.a=a;
	});
	Event.$on('transferUser',function(b){
		this.b = b;
	}.bind(this))
}
```
```js
//使用watch侦听器,监听props中的数据实现控制当前子组件内容
props: ["orderInfo"],
  data: function() {
    return {
      yetOrder: false
    };
  },
  watch: {
    orderInfo: function(newVal, oldVal) {
      // newVal 为改变后的值
      if (newVal.reserve_id && newVal.reserve_id != 0) {
        this.yetOrder = true;
      }
    }
  },
```
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
```html
<ul id="example">
    <li v-for="item in items">
        {{$index}}
        {{$key}}
    </li>
</ul>
```
现在已经移除,如果还用的话就会报错:Uncaught ReferenceError: $index is not defined;

现在这样写:
```html
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
```es6
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
```js
Vue.prototype.highlight= function (sTitle) {
  // to do
};
//然后所有组件都可以使用方法
v-html="highlight(option.title)"
```
2.使用 $options.filters
```js
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
```js
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
})
页面上：
<div>{{content}}</div>
```

### vue路由频繁切换的时候，会有部分白屏问题，下拉后恢复正常(用于同一个页面，点击导航栏切换不同路由)
```js
mounted() {
   this.setPosition(); 
},
method:{
  setPosition() {
      var oTop = $(".page-center").offset().top;
        //获取导航栏的高度，此高度用于保证内容的平滑过渡
        var martop = $('.page-center').outerHeight();
        var sTop = 0;
        // 监听页面的滚动
        $(window).scroll(function () {
            // 获取页面向上滚动的距离
            sTop = $(this).scrollTop();
            // 当导航栏到达屏幕顶端
            if (sTop >= oTop) {
                // 修改导航栏position属性，使之固定在屏幕顶端
                $(".page-center").css({ "position": "fixed", "top": "0" });
                // 修改内容的margin-top值，保证平滑过渡
                $(".page-bottom").css({ "margin-top": martop });
            } else {
                // 当导航栏脱离屏幕顶端时，回复原来的属性
                $(".page-center").css({ "position": "static" });
                $(".page-bottom").css({ "margin-top": "0" });
            }
        });
    },  
}

参考自：[https://juejin.im/post/5be92ae2e51d4572fd18c4c6](https://juejin.im/post/5be92ae2e51d4572fd18c4c6)
```

### vue中实现接口刷新，页面不刷新，这样变相的做到了返回定位到之前位置，并且不会有缓存数据：
> 目前实在没找到bug，只能在项目中出真知，手动微微无奈😔？很慌
当组件在 <keep-alive> 内被切换，它的 `activated` 和 `deactivated` 这两个生命周期钩子函数将会被对应执行。

引用上面句话，在 <keep-alive> 组件内把请求放在`activated`生命周期中，就可以做到刷新数据不刷新页面

`activated`钩子在 `keep-alive` 组件激活时调用(进入当前组件)
`deactivated`钩子在 `keep-alive` 组件停用时调用(离开当前组件)

> 在 2.2.0 及其更高版本中，activated 和 deactivated 将会在 <keep-alive> 树内的所有嵌套组件中触发。

* 如果在父组件使用了`keep-alive`，子组件生命周期方法只能使用`activated`和`deactivated`

* 因为用到了缓存，所以缓存组件中的vdom都会缓存，导致更新数据时vdom没有更新，暂时想到的办法是
`deactivated`周期加个`v-if`来清除不想缓存的vdom，`activated`周期再显示出来

### vue中利用qrcodejs2插件前端生成二维码
1.安装qrcodejs2插件，在控制台输入：
```npm
npm install qrcodejs2 --save
```
2.template or 入口文件 引入插件：
```js
import QRCode from "qrcodejs2";
```
3.html中加标签：
```html
<div id="qrcode"></div>
```
4.生成二维码：
```js
//如果是props传入的二维码地址：
props: ["qrCodeLink"],
watch: {
  qrCodeLink(newValue, oldValue) {
    //生成二维码
    new QRCode(document.getElementById("qrcode"), newValue);
  }
}
//如果是写死的，或者不是父组件传来的，直接new就好：
new QRCode(document.getElementById("qrcode"), qrCodeLink);
// 如果是点击生成二维码，会存在点一次生成一次的情况，所以要每次清除掉上次生成的二维码
document.getElementById(id).innerHTML = "";
```

### vue订单列表倒计时（这种方式适合vue单组件，即一个组件为一个订单，不需要遍历可以直接操作，父级组件已经遍历好，方便快捷暴力）
```js
let that = this;
let nowTime = new Date().getTime();
//expire_time为接口返回截止时间的时间戳
let endDate = v.expire_time * 1000 - nowTime;
// let endDate = 10000;
if (endDate > 0) {
  let stop = setInterval(() => {
    //days | hour 分别表示天和小时
    //days = Math.floor(time.t / 1000 / 60 / 60 / 24);
    //hour = Math.floor(time.t / 1000 / 60 / 60 % 24);
    let minute = Math.floor((endDate / 1000 / 60) % 60);
    let second = Math.floor((endDate / 1000) % 60);
    let min = minute < 10 ? "0" + minute : minute;
    let sec = second < 10 ? "0" + second : second;
    if (endDate <= 0) {
      //如果倒计时结束，直接改变当前订单的状态
      that.overTime = true;
      clearInterval(stop);
      return false;
    } else {
      endDate -= 1000;
    }
    that.lastTime = time.m + ":" + time.s;
  }, 1000);
} else {
  //
  that.overTime = true;
}
```

### 链接不改，版本迭代会导致再微信内浏览器中页面缓存为旧页面
详细：见[解决方法](https://www.jianshu.com/p/cce9511c0914)
其中：
设置nginx的缓存机制；直接将nginx的缓存设置成`{expires-1;}`，设置成永远不缓存；如果没有nginx，其他apache什么的通用这个方法。

### vue-resource实现vue异步请求
```html
<script src="https://cdn.staticfile.org/vue-resource/1.5.1/vue-resource.min.js"></script>
```
```js
var vm = new Vue({
  el: '#app',
  data: {
    todos: [
      { text: '学习 JavaScript' },
      { text: '学习 Vue' },
      { text: '整个牛项目' }
    ],
    tftext: "这是一段24px大小的字体"
  },
  created() {
    //get请求
    this.$http.get("/api/getapi").then(function (data) {
      // console.log(data)
    })
    //post请求
    this.$http.post("/api/postapi",
      { mobile: xxxxxxxxx },
      {
        headers: { 'Content-Type': 'application/x-www-form-urlencoded;charset=UTF-8' },
        emulateJSON: true
      }
    ).then(function (data) {
      console.log(data)
    })
  },
  mounted() {
    console.log(this.todos)
  }
})
```
* 上面例子中
get请求很好懂，如果要加参数，直接在api链接后面拼上即可，
而post中有几个注意点：
post(url, [body], [options])
1.post中第一个参数为请求的目标api url;
2.第二个参数为作为请求体发送的数据，类型可以为Object, FormData, string
3.第三个参数很多，较常用的有headers:请求头、emulateJSON:设置请求体的类型为application/x-www-form-urlencoded
4.如果要改为formdata提交，第三个参数加
{
headers: { 'Content-Type': 'application/x-www-form-urlencoded;charset=UTF-8' },
emulateJSON: true
}
即可

### vue.js源码分析
1.Object.create(null)和{}区别；
 二者都是创建一个对象，前者去掉了原型链，后者保留原型链


### 华为浏览器微信H5支付，回跳链接正确，页面显示却错误
初步分析原因可能有两个：
1.华为自带浏览器回跳未刷新页面，只是更改了路由链接，对vue路由不友好(也可能是我用了keep-alive组件，但是其他手机没问题)
2.未在微信公众后台配置回跳具体到路由参数的链接都会被拦截至首页