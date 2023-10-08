---
title: vuex记录
date: 2018-07-05 14:20:31
tags: vue
categories: vue
---


### vuex中Action和mutation的区别
Action 提交的是 mutation，而不是直接变更状态。
Action 可以包含任意异步操作。

### vuex使用注意
1.路由组件之间跳转不会刷新vuex中mutation的值，就是说，vuex会保存上次存储过的值，所以要在每次路由跳转到指定页面时，给vuex中的值初始化一次，保证不会把上次的记录的值带过来（待定验证）
2.可以直接把从vuex中取的数据传给后台，不会把vue自定义的东西带过去；

### 提交载荷
```js
// store.js中
const store = createStore({
  state: {
    count: 1
  },
  mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}
})
```
```js
store.commit('increment', {
  amount: 10
})
```
