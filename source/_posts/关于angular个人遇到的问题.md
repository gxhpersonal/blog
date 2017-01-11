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
> [详细解释：](http://blog.csdn.net/rangqiwei/article/details/38020667)


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

###ng-bind-html=""
当想要呈现的后台文本中有HTML标签的时候，使用ng-bind-html来代替ng-bind，就能正常的使用想用的HTML标签了。但要注意文本中如果使用到符号的话，注意要转义符号。
但要注意文本中如果使用到符号的话,注意要转义符号
如: (1)  大于符号（>）   ---> &gt;
      (2)  小于符号（<）   ---> &lt;
否则系统报错
要使用：$sanitize服务
在angular.module中配置sanitize服务：var myApp = angular.module('myApp', ['ngSanitize']);
<script src="../js/angular-sanitize.js"></script>

###angular自带遍历方法  angular.forEach
```javascript
var objs =[{a:1},{a:2}];
angular.forEach(objs, function(data,index,array){
        //data等价于array[index]
        console.log(data.a+'='+array[index].a);
});
```

### angularJS懒加载

引入  angular-img-lazy-load.min.js

src-lazy="{{list.ProductImgs}}"

var hzcApp = angular.module("hzc_app", ['img-lazy-load']);
> 下面这个包含在controller中
hzcApp.constant('imgLazyLoadConf', {
                tolerance: 50,
                detectElement: true
      })