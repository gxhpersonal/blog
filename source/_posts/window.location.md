---
title: window.location
date: 2016-09-14 18:07:27
tags:
categories: JS
---

### window.location
> 返回（当前页面的）整个 URL：
console.log(location.href)

返回当前 URL 的路径名：
console.log(location.pathname);

加载一个新的文档：
window.location.assign("http://www.w3school.com.cn")

  ● location.hostname 返回 web 主机的域名
  ● location.pathname 返回当前页面的路径和文件名
  ● location.port 返回 web 主机的端口 （80 或 443）
  ● location.protocol 返回所使用的 web 协议（http:// 或 https://）