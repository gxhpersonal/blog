---
title: Nuxt.js 3.8
date: 2023-11-16 09:51:54
tags: Nuxt
categories: Nuxt
---

官网：https://nuxt.com.cn/

### 配置环境变量
1.根目录下新建文件`.env.development`和`.env.production`，当然文件`.env.xxx`都行，只要和后面对应上即可；
2.文件中增加全局环境变量，例如：`PUBLIC_API_BASE=https://api.com`;
3.`nuxt.config.ts`配置文件增加配置项：
```js
export default defineNuxtConfig({
runtimeConfig: {
    apiKey: '',//默认为空字符串，在运行时使用process.env.NUXT_API_KEY自动设置，为项目域名
    public: {
       baseURL: process.env.PUBLIC_API_BASE //暴露在前端的自定义路径
    }
  },
})
```
4.引用环境变量：
```js
useRuntimeConfig().public.baseURL
```

### 配置api代理，解决跨域
```js
//这种只适合在dev环境，配置也能看出来devProxy配置，如果是预渲染测试，则此配置不行，当然服务端渲染可
export default defineNuxtConfig({
nitro: {
    devProxy: {
      "/api": {
        target: "http://192.168.1.1", // 这里是接口地址
        changeOrigin: true,
        prependPath: true,
      },
    },
    // 该配置用于服务端请求转发
    routeRules: {
      '/api/**': {
        proxy: 'http://192.168.1.1/**'
      }
    }
  },
})
//在调用接口的地方直接使用`/api`即可，不需要配置域名和ip
```

### 封装api请求方法
```js
/**
 * 网络请求方法
 * @param obj 请求参数
 * @returns 响应结果
 */
export const $http = async (obj) => {
  try {
    const res = await useFetch(
      (obj.baseURL ?? useRuntimeConfig().public.baseURL) + obj.url,
      {
        method: obj.method ?? "GET",
        query: obj?.query ?? null,
        body: obj?.body ?? null,
        onRequest({ request, options }) {
          // 设置请求报头
          options.headers = options.headers || {}
        },
        onRequestError({ request, options, error }) {
          // 处理请求错误
          console.warn('Request Error');
        },
        onResponse({ request, response, options }) {
          // 处理响应数据
          // resolve(response._data)
        },
        onResponseError({ request, response, options }) {
          message.warning('Request Error');
          // 处理响应错误
        }
      },
    )
    if (res.status.value === "success") {
      return Promise.resolve(res.data.value)
    } else {
      throw new Error(res.error)
    }
  } catch (err) {
    console.log(err)
  }
}
```
`useFetch`适合在ssr中使用，如果是在ssg中使用，用`$fetch`最佳，参数都相同，改个名即可，如果后面想转ssr，再改名就可以了

### 静态资源
静态资源（如图片）放在`public`目录下，没有新建一个，引用时直接`/demo.png`即可。

### useState全局共享状态
```js
const useCityName = (id) => {
  return useState("commonCityName", () => id);
};
//改变useState中的值，使用 .value 属性赋值；
const change = () => {
  const cityName = useCityName();
  cityName.value = v.city_name;
}
```

### 在不同端获取数据
```js
/* 此调用在水合之前执行 */
const { article } = await useFetch('api/article')

/* 此调用仅在客户端执行 */
const { pending, data: posts } = useFetch('/api/comments', {
  lazy: true,
  server: false
})
```
要注意的是，如果使用的预渲染，水合之前执行的数据，img图片会有缓存，所以如果是经常变动的数据，不要使用这种方式请求数据；

### 服务端渲染（ssr）相关配置
1. 新建文件`ecosystem.config.js`，并填写以下代码：
```js
module.exports = {
  apps: [
    {
      name: 'NuxtAppName',
      port: '8921',
      path : "服务器IP",
      exec_mode: 'cluster',
      instances: 'max',
      script: './.output/server/index.mjs'
    }
  ]
}
```
2. 安装pm2
```bash
npm install -g pm2
```
3. 在服务器项目目录下运行
```bash
pm2 start nuxt
```