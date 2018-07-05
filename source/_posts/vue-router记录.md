---
title: vue-router记录
date: 2018-07-05 14:21:09
tags: vue
categories: vue
---


### vue-router
<router-link>组件使用
```
<router-link v-if="!item.isSubmit" :to="'claimOrder?orderNo='+item.orderno+'&caroid='+$route.query.caroid+'&id='+item.id+'&Ins='+$route.query.Ins" class="btn">继续上传</router-link>
```
<router-link> 组件比起写死的 `<a href="..."></a>`会好一些，理由如下：
无论是 HTML5 history 模式还是 hash 模式，它的表现行为一致，所以，当你要切换路由模式，或者在 IE9 降级使用 hash 模式，无须作任何变动。
在 HTML5 history 模式下，router-link 会守卫点击事件，让浏览器不再重新加载页面。
当你在 HTML5 history 模式下使用 base 选项之后，所有的 to 属性都不需要写（基路径）了。

### 路由懒加载
> 非懒加载写法：
```
import Index from '@/page/index/index';
export default new Router({  
    routes: [    
        { 
            path: '/', 
            name: 'Index',     
            component: Index 
        }
    ]
})
```
> 路由懒加载写法：
```
export default new Router({
  routes: [    
        { 
            path: '/', 
            name: 'Index', 
            component: resolve => require(['@/view/index/index'], resolve) 
        }
   ]
})
```

