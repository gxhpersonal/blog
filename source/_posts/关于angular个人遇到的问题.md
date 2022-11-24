---
title: 关于angular个人遇到的问题（差不多过去式了）
date: 2016-09-14 15:50:50
tags: angular
categories: angular
---

###动态插入的标签元素带有angular语法不执行解决
```
var uploadInfo = '上传成功！<a ng-click="quitTo('customer')">点击</a>跳转到客户列表界面';
angular.element('.modal-body').append(uploadInfo);
```
将uploadInfo变量通过$compile进行处理，
var ele = $compile(uploadInfo)($scope);
angular.element('.modal-body').append(ele);

###依赖注入
AngularJS提供了一种非常简单的解决方法，即将依赖作为一个数组传入，数组的最后一个元素是一个函数，所有的依赖项作为它的参数。
```
app.controller('MainCtrl', ['$scope', '$timeout', function($scope, $timeout){
    $timeout(function(){
        console.log($scope);
    }, 1000);
}]);
```

### 获取input焦点(自定义一个指令)
```javascript
var myApp = angular.module('myApp',[]);
myApp.directive('setFocus', function(){
return function(scope, element){
element[0].focus();
};
});

```

### ng-repeat遍历数组
> ng-repeat遍历数组时，数组中有2个以上的相同数字，需要在 v in xxx 后面加上 track by $index；
> [详细解释：](http://blog.csdn.net/rangqiwei/article/details/38020667)

### ng-repeat遍历对象
> ng-repeat="(key,value) in feesWays"
> 这种方式可以把接口数据对象格式的字段分离成键和值；

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
                $scope.overhtml = "活动已结束！";
                $interval.cancel(stop);
                return false;
            } else {
                activityendtime = (time.t - 1000);
            }
            $scope.Residualtime = time.d + ":" + time.h + ":" + time.m + ":" + time.s;
        };
        count();
        var stop = $interval(count, 1000);
    }
    $scope.daojishi（$scope.activityendtime）
```

### angularJS全局API
> angular.lowercase()	转换字符串为小写
angular.uppercase()	转换字符串为大写
angular.isString()	判断给定的对象是否为字符串，如果是返回 true。
angular.isNumber()	判断给定的对象是否为数字，如果是返回 true。

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
```
var hzcApp = angular.module("hzc_app", ['img-lazy-load']);
> 下面这个包含在controller中
hzcApp.constant('imgLazyLoadConf', {
                tolerance: 50,
                detectElement: true
      })
```

### directive
2 . 在AngularJS中，操作DOM一般在指令中完成，事件监听机制是在对于已经静态生成的dom绑定事件，而如果在指令中动态生成了DOM节点，动态生成的节点不会被JS事件监听
```
angular.module('myapp',[])
.directive('myText',function($compile){
    var template:'<div ng-click="hello()">Hi everyone</div>',
    return{
        restrict:'A',
        link:function(scope,ele,attr){
             ele.on("click", function() {
                scope.$apply(function() {
                    var content = $compile(template)(scope);
                    element.append(content);
               })
            });
        }
    }
}
```

### $timeout $apply
$timeout会帮你调用$apply()，让你不需要手动地调用它
$apply会吧我们的代码wrapped到了$scope.$apply()中，它会自动触发$rootScope.$digest()，从而让watchers被触发用以更新view

### ng-click方法函数中的this指向问题
> this指向当前controller中的$scope
> 比如：this.value = $scope.value;
```
ng-click中的this的特性在ng-repeat中非常好用。
比如用ng-repeat遍历出一个列表：
<div ng-click="change()" ng-repeat="item in items">{{item.value}}</div>
然后你想给他们绑定一个统一的函数change()方法，点击之后只会把自己的值改变，此时this就是指向当前的子scope：
$scope.change = function(){
    this.item.value = 'new value';
    //由于是ng-repeat出来的，所以这里this指向的scope是上面那个案例中$scope的子集。
    //即this.$parent === $scope;
}
```

### angular.cope()复制数组或对象


### ng-bind-html绑定接口获取的html，数据处理
```
.filter('to_trusted', ['$sce', function ($sce) {
    return function (text) {
      return $sce.trustAsHtml(text);
    }
  }])
  .directive('compileHtml', function ($compile) {
    return {
      restrict: 'A',
      replace: true,
      link: function (scope, ele, attrs) {
        scope.$watch(function () { return scope.$eval(attrs.compile); },
          function (html) {
            ele.html(html);
            $compile(ele.contents())(scope);
          });
      }
    };
  });
```

## $http
angular提供了$http服务来同服务端进行通信，$http服务对浏览器的XMLHttpRequest对象进行了封装，让我们可以以ajax的方式来从服务器请求数据。
$http服务是一个接受一个参数的函数，参数的类型是对象，用来配置生成的http的请求，该函数返回一个promise对象（promise对象是一个构造函数ES6的对象）

```
var promise = $http({
  method:'GET',
  url:'/api/user.json'
});
promise.then(
	function(resp){
		//success状态为resolved时调用（即异步操作成功）
	}, function(resp){
		//failure可选，状态为rejected时调用（失败）
	})
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

### $http配置为form data 提交:
```js
$http({
        method: 'POST',
        url: '/user/',
        /*$.param() 方法创建数组或对象的序列化表示形式，
        例如：{mobile:134****2345,email:a@qw.com} 改为 mobile:134****2345&email=a@qw.com*/
        data: $.param($scope.formData), 
        // 设置post为formdata表单提交，而不是默认的json提交
        headers: { 'Content-Type': 'application/x-www-form-urlencoded;charset=utf-8' } 
    }).success(function(data) {
        console.log(data);
    });
```

### 获得ng-model里的value
ng-model="name", $scope.name拿不到值是因为原型继承，具体可以百度，解决方法是使用对象属性取值，ng-model="model.name"; $scope.model ={}; 
$scope.model.name这个时候就是双向绑定了。

### 指令写图片懒加载
先用一个小的图片占位，图片加载完毕显示需要加载的图片；
html：src为默认显示的图片，lazy-src为需要懒加载的图片
```html
<img image-lazy-load src=""  lazy-src="default.jpg" alt=""/>
```
```js
app.directive('imageLazyLoad', function() {
   return {
       // A = attribute, E = Element, C = Class and M = HTML Comment
       restrict: 'A',
       link: function(scope, ele, attrs){
           ele[0].src = 'default.jpg';   //默认
           ele[0].onload =function (ev) {  //图片onload完毕
               ele[0].src = attrs.lazySrc;
           }
       }
   };
});
```