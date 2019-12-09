---
title: Nuxt.js预渲染
date: 2019-12-05 09:51:54
tags: Nuxt
categories: Nuxt
---

### nuxt基于vuejs的seo优化

1.css单独打包
在nuxt.config.js配置中加下面代码：
```json
build: {
    extractCSS: true,
  }
```

2.html中TDK自定义
page目录下的.vue文件中加下面代码：
```js
export default {
  head() {
		return {
			title: "家博会_智能家居_京东森图里官网",
			meta: [
				{
					hid: "description",
					name: "description",
					content: "京东森图里家博会由北京展览馆、京东华墨展览联合打造的时尚、智能、科技、现代的家居消费类展会，采用线上线下"
				},
				{
					hid: "keywords",
					name: "keywords",
					content: "家博会_智能家居展_家具展会_京东森图里家博会"
				}
			]
		};
	}
}

```

3.修改局域网配置
在package.json中新添加
```
"config": {
    "nuxt": {
      "host": "127.0.0.1",
      "port": "8080"
    }
  }
```