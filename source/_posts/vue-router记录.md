---
title: vue-router记录
date: 2018-07-05 14:21:09
tags: vue
categories: vue
---


### vue-router
<router-link>组件使用
```html
<router-link v-if="!item.isSubmit" :to="'claimOrder?orderNo='+item.orderno+'&caroid='+$route.query.caroid+'&id='+item.id+'&Ins='+$route.query.Ins" class="btn">继续上传</router-link>
```
<router-link> 组件比起写死的 `<a href="..."></a>`会好一些，理由如下：
1.无论是 HTML5 history 模式还是 hash 模式，它的表现行为一致，所以，当你要切换路由模式，或者在 IE9 降级使用 hash 模式，无须作任何变动。
2.在 HTML5 history 模式下，router-link 会守卫点击事件，让浏览器不再重新加载页面。
3.当你在 HTML5 history 模式下使用 base 选项之后，所有的 to 属性都不需要写（基路径）了。

### 路由懒加载
> 非懒加载写法：

```js
import HelloWorld from '@/components/HelloWorld'
export default new VueRouter({
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      component: HelloWorld,
      meta:{
        title:"svip"
      }
    }
  ]
})
```
> 路由懒加载写法：

1.利用ES6 promise和webpack2动态import实现异步组件懒加载：

```js
const Foo = () => import('./Foo.vue')
```

2.利用webpack1中的 `require.ensure()`
```js
//require.ensure() 是webpack中用来代码分割,把每个js大包打包成不同文件，缩小文件体积；
const commentOrder = resolve => {
    require.ensure([], () => resolve(require('../components/userComment/comment.vue')), '/commentOrder')
}
export default new Router({
  routes: [    
        { 
            path: '/commentOrder', 
            name: 'commentOrder', 
            component: {commentOrder:commentOrder}
        }
   ]
})
```
文中提到的require.ensure()详细见<http://www.css88.com/doc/webpack2/guides/code-splitting-require/>

### 路由跳转时更改页面title
1.router.js文件中给每个path添加meta:{}字段，如下：
```js
export default new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      children: [
        {
          path: 'bar',
          component: Bar,
          // a meta field
          meta: { title: '详情页' }
        }
      ]
    }
  ]
})
```
2.js入口文件main.js中添加代码：
```js
router.beforeEach((to, from, next) => {
  if (to.meta.title) {
      document.title = to.meta.title
    // this route requires auth, check if logged in
    // if not, redirect to login page.
  }
  next() // 确保一定要调用 next()
})
```

### this.$router.xxx访问路由

