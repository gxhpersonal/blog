---
title: angular中的请求
date: 2016-11-28 15:14:32
tags: angular
categories: angular
---
## $http
angular提供了$http服务来同服务端进行通信，$http服务队浏览器的XMLHttpRequest对象进行了封装，让我们可以以ajax的方式来从服务器请求数据。
$http服务是一个接受一个参数的函数，参数的类型是对象，用来配置生成的http的请求，该函数返回一个promise对象（关于promise规范，可以看看这篇文章）

```
var promise = $http({
  method:'GET',
  url:'/api/user.json'
});
promise.then(function(resp){}, function(resp){})
```


$http请求的配置对象
$http()接受的配置对象可以包含以下属性:

> method:http请求方式，可以为GET,DELETE,HEAD,JSONP,POST,PUT
url:字符串，请求的目标
params:字符串或者对象，会被转换成为查询字符串追加的url后面
data:在发送post请求时使用，作为消息体发送到服务器
headers:一个列表，每个元素都是一个函数，返回http头
xsrfHeaderName(字符串)：保存XSFR令牌的http头的名称
xsrfCookieName:保存XSFR令牌的cookie名称
transformRequest:函数或者函数数组，用来对http请求的请求体和头信息进行转换，并返回转换后的结果。
transformResponse:函数或者函数数组，用来对http响应的响应体和头信息进行转换，并返回转换后的结果。
cache:布尔类型或者缓存对象，设置之后angular会缓存get请求。
timeout:数值，延迟请求
responseType：字符串，响应类型。可以为arraybuffer, blob,document,json, text, moz-blob, moz-chunked-text, moz-chunked-arraybuffer
$http请求的响应对象
angular传递给then方法的响应对象包括以下几个属性
data:转换之后的响应体
status:http响应状态码
headers:头信息
config:生成原始请求的设置对象
statusText:http响应状态的文本
