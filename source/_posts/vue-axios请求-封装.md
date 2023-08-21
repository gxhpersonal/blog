---
title: vue-axios请求+封装
date: 2019-03-26 11:40:19
tags: vue
categories: vue
---

### axios实现vue异步请求 + 封装
> 安装axios && vue-axios : npm install --save axios vue-axios
#### 1.封装不同类型的请求（get,post..）,`api/helpers.js`配置:
```js
import Vue from "vue";
import axios from "axios";
import VueAxios from "vue-axios";
Vue.use(VueAxios, axios);
class request {
    constructor() {
        //定义请求接口地址对象，一个是开发环境地址，另一个就是线上地址
        this.urlMap = {
            development: '/dev',
            production: '/product'
        }
    }
    //get请求
    Get(obj) {
        //封装一层promise，一来可以把接口数据.then出去，二来解决嵌套请求的回调问题
        return new Promise((resolve, reject) => {
            //发起axios请求
            Vue.axios(
                //利用ES5的浅拷贝Object.assign自动增删传入的对象
                Object.assign({}, {
                    method: "get",
                    //设置请求头来区分请求数据类型，formdata || json
                    headers: {
                        "Content-Type": "application/x-www-form-urlencoded;charset=UTF-8"
                    },
                    //`baseURL` 将自动加在 `url` 前面，除非 `url` 是一个绝对 URL
                    baseURL: this.urlMap[process.env.NODE_ENV]
                }, obj)
            ).then((res) => {
                resolve(res.data)
            }).catch((e) => {
                reject(e)
            })
        })
    }
    //post请求
    Post(obj) {
        //封装一层promise，一来可以把接口数据.then出去，二来解决嵌套请求的回调问题
        return new Promise((resolve, reject) => {
            Vue.axios(
                Object.assign({}, {
                    method: "post",
                    headers: {
                        "Content-Type": "application/x-www-form-urlencoded;charset=UTF-8"
                    },
                    baseURL: this.urlMap[process.env.NODE_ENV],
                    // `transformRequest` 允许在向服务器发送前，修改请求数据
                    // 只能用在 'PUT', 'POST' 和 'PATCH' 这几个请求方法
                    // 后面数组中的函数必须返回一个字符串，或 ArrayBuffer，或 Stream
                    transformRequest: [(data) => {
                        // 对 data 进行form-data处理，这个只针对后端还需要formData格式数据，如果是json则简单许多，无需区分请求方式
                        let rData = ''
                        for (let i in data) {
                            rData += encodeURIComponent(i) + '=' + encodeURIComponent(data[i]) + '&'
                        }
                        //字符串最后多了个&符号，截取掉
                        return rData.substr(0,rData.length-1)
                    }]
                }, obj)
            ).then((res) => {
                resolve(res.data)
            }).catch((e) => {
                reject(e)
            })
        })
    }
}
export {
    request
}
```

#### 2.具体某接口的请求封装：api/indexApi.js配置：
```js
import {request} from '../js/helper';
class homeApi extends request {
    //构造器
    constructor() {
        //调用super方法，使子类自己的this对象得到父类同样的实例属性和方法
        super()
    }
    //其中的this指向子类的实例，因为上面已经完成对父类的继承
    getList(data, handle) {
        this.Get({ url: '/List?page=' + data.pageId }).then((res) => {
            handle(res)
        }).catch((e) => {

        })
    }
    getOther(data, handle) {
        //data:'mobile='+data.mobile是form-data数据类型，{data:{mobile:xxxxxxx}}为json类型
        this.Post({ url: '/getOther' ,data:'mobile='+data.mobile}).then((res) => {
            handle(res)
        }).catch((e) => {

        })
    }
}
export {
    homeApi
}
```

#### 3.封装后的具体应用：app.vue:
```html
<template>
  <div class="login-wrap">121212</div>
</template>

<script>
import {homeApi} from "../assets/api/indexApi";
const Api = new homeApi()
export default {
  data() {
    return {};
  },
  mounted() {
    //get请求
    Api.getList(
      {
        pageId: 1
      },
      res => {
        console.log(res)
      }
    );
    //post请求
    Api.getOther(
      {
        mobile:XXXXXXXX
      },
      (res)=>{
        console.log(res)
      }
    )
  }
};
</script>
```

* 题外：
export也可以写成export default XXX,
如：```export {homeApi}  = export default homeApi```
引用时写成：
```import homeApi from "../assets/api/indexApi";```
`export`可以写多个，`export default`只能写一个，所以这就是为什么`export`输出的是对象，而`export default`输出的是变量或者类，
然后引用`import`的时候也同理使用`export`输出时，引用的是对象，而使用`export default`输出时，引用的是变量或者类
`export`可以只输出需要用到的方法，`export default`则是全部输出

## 重中之重：有些浏览器不支持header头设置为*全部，否则会报network error，请求失败，要具体到每个参数