---
title: Nuxt.js预渲染
date: 2019-12-05 09:51:54
tags: Nuxt
categories: Nuxt
---

## 个人认为这个框架有点鸡肋，起初以为它可以做到SSR，事实too young too naive,这个框架还是相当于封装了个prerender-spa-plugin插件，增加了一些独有的特性和方法，要想做到SSR还是得依赖node；
### nuxt基于vuejs的seo优化

一、css单独打包
在nuxt.config.js配置中加下面代码：
```json
build: {
    extractCSS: true,
  }
```

二、html中TDK自定义
page目录下的.vue文件中加下面代码：
```js
export default {
	head() {
		return {
			title: "title",
			meta: [
				{
					hid: "description",
					name: "description",
					content: "desc"
				},
				{
					hid: "keywords",
					name: "keywords",
					content: "key"
				}
			]
		};
	}
}
```

三、修改局域网配置
在package.json中新添加
```json
"config": {
    "nuxt": {
      "host": "127.0.0.1",
      "port": "8080"
    }
  }
```

四、插件引用
以生成二维码插件为例：
1.在`plugins`目录下新建`qrcode.js`文件:
```js
import Vue from 'vue'
import qrcode from "qrcodejs2";

Vue.prototype.$qrcode = qrcode;
```
2.在`nuxt.config.js`配置文件中：
```js
plugins: [
	{ src: '~/plugins/qrcode.js', ssr: false },
]
```
这样就可以在全局.vue中使用了，`new this.$qrcode()`

五、环境配置
1.`nuxt.config.js`文件中添加配置项：
```js
env: {
	BASE_URL: process.env.BASE_URL,
	NODE_ENV: process.env.NODE_ENV
},
```
2.`package.json`文件中修改：
```json
"scripts": {
	"dev": "cross-env BASE_URL=http://xxx NODE_ENV=development nuxt",
	"test": "cross-env BASE_URL=http://xxx NODE_ENV=production nuxt generate",
	"generate": "cross-env BASE_URL=http://xxx NODE_ENV=production nuxt generate"
},
```
这样，本地、测试、生产环境就都有了，执行对应命令即可如：npm run dev, npm run test, npm run generate

3.在axios等请求中就可以使用不同的`process.env.BASE_URL`来使用接口域名

六、asyncData 方法
* 你可能想要在服务器端获取并渲染数据。Nuxt.js添加了asyncData方法使得你能够在渲染组件之前异步获取数据。

七、css按需引入
通过阅读 nuxt 源码发现了端倪，发现在打包时将所有的公共css通过 splitChunks 进行分组，由于项目中组件都是动态引入的，这里直接在 nuxt.config.js 中修改webpack打包参数，将 splitChunks.cacheGroups.styles 配置删除，使用默认的 chunks: async 配置，即可实现按需引入。