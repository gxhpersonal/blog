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

4.插件引用
