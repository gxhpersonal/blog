---
title: 关于angular个人遇到的问题
date: 2016-09-14 15:50:50
tags:
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

### 数组和对象的解决
> 如果在一个大的数组里有很多对象，想要获取到对象身上的某一个字段，就需要 var一个变量，把需要的字段以“data[0].xxx.xxx”的形式取到赋给定义好的变量，然后再用 $scope.xxx 的方法把这个变量的值绑定到angular中

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
        var time = $scope.time = {};
        console.log(activityendtime)
        time.endTime = new Date($scope.activityendtime);
        console.log(time.endTime);
        var count = function () {
            time.nowTime = new Date();
            //计算剩下毫秒数
            time.t = time.endTime.getTime() - time.nowTime.getTime();
            time.d = Math.floor(time.t / 1000 / 60 / 60 / 24);
            time.h = Math.floor(time.t / 1000 / 60 / 60 % 24);
            time.m = Math.floor(time.t / 1000 / 60 % 60) < 10 ? '0' + Math.floor(time.t / 1000 / 60 % 60) : Math.floor(time.t / 1000 / 60 % 60);
            time.s = Math.floor(time.t / 1000 % 60) < 10 ? '0' + Math.floor(time.t / 1000 % 60) : Math.floor(time.t / 1000 % 60);
            $scope.Residualtime = time.d + ":" + time.h + ":" + time.m + ":" + time.s;
            if (time.t <= 0) {
                $scope.endShow = true;
                $scope.endActivity = true;
                if (time.h < 0) {
                    time.h = '0';
                    time.m = '00';
                    time.s = '00';
                }
                $(".count-down").hide();
            }
        };

        count();
        var stop = $interval(count, 1000);
        $scope.$on('$destroy', function () {
            $interval.cancel(stop);
        });
```

### 字符串拼接
> angular中需要拼接字符串和变量时，用ng-xx方法，“”中写 字符串{{变量}} ，就会解析为字符串