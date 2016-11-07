---
title: 关于angular个人遇到的问题
date: 2016-09-14 15:50:50
tags:
categories: angular
---
### 关于ng-if
> 当一个元素需要隐藏和显示时，用ng-if的话，会出现其中包含的元素身上的方法无法正确调用的现象，所以，当元素中有方法时，最好使用 ng-show 和 ng-hide

### 获取input焦点
```javascript
var myApp = angular.module('myApp',[]);
myApp.directive('setFocus', function(){
return function(scope, element){
element[0].focus();
};
});

```

### ng-repeat遍历数组
> ng-repeat遍历数组时，需要在 v in xxx 后面加上 track by $index；

### ng-repeat和ng-options区别
> ng-repeat 指令是通过数组来循环 HTML 代码来创建下拉列表，但 ng-options 指令更适合创建下拉列表，它有以下优势：
使用 ng-options 的选项的一个对象， ng-repeat 是一个字符串。

### ng-switch:根据选中的值显示对应部分:
> 对应的子元素使用 ng-switch-when 指令，如果匹配选中选择显示，其他为匹配的则移除。
你可以通过使用 ng-switch-default 指令设置默认选项，如果都没有匹配的情况，默认选项会显示。


### 输出当前点击的对象属性值
```html
<!doctype html>
<html lang="en" ng-app="toggleList">
<head>
	<meta charset="UTF-8">
	<title></title>
	<style type="text/css">
		.error {
			background-color: red;
		}
		.warning {
			background-color: yellow;
		}
	</style>
</head>
<body>
	<div ng-controller="test" class="">
		<a href="javascript:void(0)" id="js_link" ng-click="showMe($event)">
			<span>点击我</span>
		</a>
	</div>
	<script src="http://cdn.bootcss.com/jquery/2.1.1/jquery.min.js"></script>
	<script src="http://cdn.bootcss.com/angular.js/1.3.20/angular.min.js"></script>
	<script src="http://cdn.bootcss.com/angular.js/1.3.20/angular-animate.min.js"></script>
	<script type="text/javascript">
		var app = angular.module('toggleList', ['ngAnimate']);
		app.controller("test", ["$scope", function ($scope) {
			$scope.showMe = function(event){
				console.log($(event.currentTarget).attr("id"))
			}
		}]);


	</script>
</body>
</html>
```

### 跳转页面
> $scope.tiaozhuan = function(number){
window.location.href="/xxx/xxx   (如果想要传ID或什么东西过去)  ?id=" + number;
}

### 倒计时
```javascript
        //后台会传一个时间戳给你，只需要把这个时间戳转换为倒计时就行
       $scope.daojishi = function (activityendtime) {
        var time = $scope.time = {};
        var count = function () {
            //计算剩下毫秒数
            time.t = activityendtime;
            time.d = Math.floor(time.t / 1000 / 60 / 60 / 24);
            time.h = Math.floor(time.t / 1000 / 60 / 60 % 24);
            time.m = Math.floor(time.t / 1000 / 60 % 60) < 10 ? '0' + Math.floor(time.t / 1000 / 60 % 60) : Math.floor(time.t / 1000 / 60 % 60);
            time.s = Math.floor(time.t / 1000 % 60) < 10 ? '0' + Math.floor(time.t / 1000 % 60) : Math.floor(time.t / 1000 % 60);
            if (time.t <= 0) {
                $scope.endShow = true;
                $scope.overhtml = "活动已结束！";
                $scope.endActivity = true;
                $scope.timeOver = true;
                if (time.h < 0) {
                    time.h = '0';
                    time.m = '00';
                    time.s = '00';
                }
                $(".count-down").hide();
            } else {
                activityendtime = (time.t - 1000);
            }
            $scope.Residualtime = time.d + ":" + time.h + ":" + time.m + ":" + time.s;
        };
        count();
        var stop = $interval(count, 1000);
        $scope.$on('$destroy', function () {
            $interval.cancel(stop);
        });
    }
    $scope.daojishi（$scope.activityendtime）
```

### 字符串拼接
> angular中需要拼接字符串和变量时，用ng-xx方法，“”中写 字符串{{变量}} ，就会解析为字符串

### angularJS全局API
> angular.lowercase()	转换字符串为小写
angular.uppercase()	转换字符串为大写
angular.isString()	判断给定的对象是否为字符串，如果是返回 true。
angular.isNumber()	判断给定的对象是否为数字，如果是返回 true。