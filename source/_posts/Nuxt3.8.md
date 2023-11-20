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