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

proxy并不比Object.definProperty更快，Object.definProperty只能监听对象属性的改变，而proxy不止可以监听对象属性的改变，还能监听对象属性的增加和删除

### 重写vDom

### 生命周期改变
`beforeDestroy`生命周期选项被重命名为`beforeUnmount`
`destroyed`生命周期选项重命名为`unmounted`

### watch监听路由
```vue
<script setup>
//watch 的第一个参数可以是不同形式的“数据源”：它可以是一个 ref (包括计算属性)、一个响应式对象、一个 getter 函数、或多个数据源组成的数组：
//如果监听路由，则都不满足，所以这里需要用一个返回该属性的 getter 函数：
const route = useRoute();
watch(() => route.path, (path) => {
  console.log(path)
});
</script>
```
