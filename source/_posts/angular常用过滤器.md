---
title: angular常用过滤器
date: 2016-09-27 11:30:40
tags:
categories: angular
---
## 一。表达式中添加过滤器
#### 过滤器可以通过一个管道字符（|）和一个过滤器添加到表达式中。

> 1. uppercase 过滤器将字符串格式化为大写：
```html
<p>姓名为 {{ lastName | uppercase }}</p>
```
> 2. lowercase 过滤器将字符串格式化为小写：
> 
```html
<p>姓名为 {{ lastName | lowercase }}</p>
```
> 3. currency 过滤器将数字格式化为货币格式：
```html
<p>总价 = {{ (quantity * price) | currency }}</p>
```
## 二。向指令添加过滤器
> 1. orderBy 过滤器根据表达式排列数组：
```html
<li ng-repeat="x in names | orderBy:'country'">
```
> 2. filter 过滤器从数组中选择一个子集：
```html
$scope.names = [
       {'name':'jack','country':'china','sex':'man'},
       {'name':'nick','country':'austrilear','sex':'woman'},
       {'name':'rose','country':'rouma','sex':'man'}
    ];
  })

<p><input type="text" ng-model="test"></p>
<ul>
  <li ng-repeat="x in names | filter:test | orderBy:'country'">
    {{ (x.name | uppercase) + ', ' + x.country }}
  </li>
</ul>
```

###limitTo过滤器：用来控制显示在页面的数量
ng-repeat:v in data | limitTo:15