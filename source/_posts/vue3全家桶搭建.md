---
title: vue3全家桶搭建
date: 2021-06-04 10:56:18
tags: vue
categories: vue
---

### 本项目用到的框架和工具：vite2 + vue3 + vue-router4 + vuex4 + TS

#### 1.搭建 Vite + Vue 项目
> Vite 需要 Node.js 版本 >= 12.0.0。
```bash
# npm 6.x
npm init @vitejs/app my-vue-app --template vue

# 如果用ts的话：
npm init @vitejs/app my-vue-app --template vue-ts

# npm 7+, 需要额外的双横线：
npm init @vitejs/app my-vue-app -- --template vue
```
* `npm -v`查看npm版本

#### 2.配置路由
```base
npm install vue-router@4 --save
```

* 在`src`目录下新建router文件夹，新建`index.ts`路由配置文件:
```js
import { createRouter, createWebHistory, RouteRecordRaw } from 'vue-router';
// 这里还是用路由懒加载方式
const Home = () => import("../src/components/Home/Index.vue");
const Detail = () => import("../src/components/Detail/Index.vue");
const notFound = () => import("../src/components/notFound.vue");
const routes: Array<RouteRecordRaw> = [
  {
    path: "/",
    name: "Home",
    meta: {
      title: "首页"
    },
    component: Home
  },
  {
    path: "/detail",
    name: "Detail",
    meta: {
      title: "详情页"
    },
    component: Detail
  },
  {
    // 这里不能直接使用vue-router3.x的*通配符做path了
    // vue-router4.x将匹配所有内容并将其放在 `$route.params.pathMatch` 下
    path: "/:pathMatch(.*)*",
    name: "notFound",
    meta: { title: "404" },
    component: notFound
  }
]
const router = createRouter({
  // 用最舒服的H5 history模式
  history: createWebHistory(), //应用托管在根目录下，如：给出的网址为 `https://example.com/`
  history: createWebHistory('/folder/'), //应用托管在子目录下，如：给出的网址为 `https://example.com/folder/`
  routes // `routes: routes` 的缩写,ES6语法糖
});
export default router;
```

* 在`main.ts`中：
```js
//创建并挂载根实例，使整个应用支持路由
import { createApp } from 'vue'
import App from './App.vue'
import router from "../router/index";

createApp(App).use(router).mount('#app')
```

* 在`App.vue`入口组件中：
```html
<template>
	<div>
    <!-- 使用`router-view`标签匹配首页 -->
    <router-view></router-view>
	</div>
</template>
```

tips:
如果部署的项目在子目录下，则相应的需要修改`vite.config.ts`文件中的base路径：
```ts
// vite.config.ts
export default defineConfig({
  plugins: [vue()],
  base:"/folder/",
  server: {
    //用本机IP启动项目，在相同网络下可以直接用手机调试
    host: '192.168.9.136',
    port: 8082
  }
})
```

### 3.配置vuex状态管理模式+库

```bash
# 安装vuex4.x
npm install vuex@next --save
```

* 和router相同，在src目录下新建`store`文件夹，并新建`index.ts`文件：
```js
// 让我们把vuex的5个核心概念同时回顾下，一步到位
import { createStore } from "vuex";
const moduleA = {
  state: () => ({
    count: 0,
    list: [{ id: 1, name: "nick", age: "18", isShow: true }, { id: 2, name: "mack", age: "19", isShow: false }]
  }),
  getters: {
    formatList(state:any) {
      return state.list.filter((v: { isShow: any; }) => v.isShow)
    }
  },
  mutations: {
    increment(state:any, data:any) {
      console.log(state, data)
      state.count = data.amount;
    }
  },
  actions: {
    incrementAsync(store:any, data:any) {
      setTimeout(() => {
        store.commit('increment', data)
      }, 2000);
    }
  }
}
const store = createStore({
  modules: {
    a: moduleA
  }
})
export default store
```

* 和`router`相同，在`main.ts`中：
```js
import store from "../store/index";
createApp(App).use(store).mount('#app')
// 这样就可以全局访问到store仓库
// 因为我们用了module核心概念，所以获取store中的数据：this.$store.state.a.count
```
>需要注意的是Vuex 没有为 `this.$store` 属性提供开箱即用的类型声明，因为我们项目用的ts，所以需要先声明自定义的[模块补充](https://www.tslang.cn/docs/handbook/declaration-merging.html)（这样只会编辑器警告，代码还是可以正常运行的，但是警告会看着很难受，所以必须解决）

所以在`src`目录下新建`vuex.d.ts`文件：
```ts
import { ComponentCustomProperties } from 'vue'
import { Store } from 'vuex'

declare module '@vue/runtime-core' {
  // 声明自己的 store state
  interface State {
    // 因为我们上面使用了vuex的module，所以直接声明module名就行
		a: any
  }

  // 为 `this.$store` 提供类型声明
  interface ComponentCustomProperties {
    $store: Store<State>
  }
}
```

### 4.配置环境变量
只要是软件，必须有api，有api必须有环境区分
和vue-cli 4.x类似，我们需要在和`package.json`同级目录下新建`.env.dev`、`.env.test`和`.env.prod`文件来区分开发，测试和正式环境；
然后在对应的文件中加入对应的`base_url`等值；
比如在`.env.prod`文件中：
```env
NODE_ENV=prod
VITE_APP_BASE_API = "api.prod.com"
```
在项目的任意位置就可以通过`import.meta.env`获取到环境变量信息；
当然我们做环境区分的目的还是要自动打不同环境的包，所以在`package.json`文件中：
```json
"scripts": {
  "dev": "vite --mode dev",
  "test": "vite build --mode test",
  "build": "vite build --mode prod",
  "serve": "vite preview"
},
```
例：打测试环境的包，只需要执行`npm run test`即可；

## 5.提升开发效率，明确dom结构，当然少不了css预处理器，vite构建工具已经非常友好的集成了预处理器，只需安装依赖：
```bash
# .scss and .sass ,这里只举一个sass例子
npm install -D sass
```
* 如果有单独引入`.scss`, `.sass`, `.less`, `.styl` 和 `.stylus`文件,或者只是在vue单文件组件中使用：
```html
<style lang="scss" scoped>
.home-wrap {
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
	.btn {
		margin-top: 2rem;
	}
}
</style>
```

### 因为写这个是为了开箱即用，所以很多东西都是根据实际业务来的，没有全面化，写的会很具体，有针对性，有后续再补充