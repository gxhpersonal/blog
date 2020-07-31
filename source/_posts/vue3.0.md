---
title: VUE3.0
date: 2019.3.13 9:00
tags: vue
categories: vue
---

### 数据监听机制 ES6 Proxy
目前，Vue 的反应系统是使用 Object.defineProperty 的 getter 和 setter。 
但是，Vue 3 将使用 ES2015 Proxy 作为其观察者机制。 这消除了以前存在的警告，使速度加倍，并节省了一半的内存开销。
为了继续支持 IE11，Vue 3 将发布一个支持旧观察者机制和新 Proxy 版本的构建。


### 重写vDom