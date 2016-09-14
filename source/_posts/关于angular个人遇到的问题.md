---
title: 关于angular个人遇到的问题
date: 2016-09-14 15:50:50
tags:
---
## 关于ng-if
> 当一个元素需要隐藏和显示时，用ng-if的话，会出现其中包含的元素身上的方法无法正确调用的现象，所以，当元素中有方法时，最好使用 ng-show 和 ng-hide

## 获取input焦点
```javascript
var myApp = angular.module('myApp',[]);
    myApp.directive('setFocus', function(){
          return function(scope, element){
            element[0].focus();
          };
    });

```

## ng-repeat遍历数组
> ng-repeat遍历数组时，需要在 v in xxx 后面加上 track by $index；