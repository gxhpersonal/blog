---
title: ES6
date: 2017-02-06 09:34:53
tags:
categories: JS
---

### let  const
let和var类似，let必须先声明后使用，否则会报错；
let声明变量，const声明常量；
```
function kuai(){
  let n = 5;
  if(true){
    let n = 10;
  }
  console.log(n)
}
kuai()
```
上面代码说明let有块级作用域，不同块级相同变量不受影响；

### 顶层对象的属性和全局变量关系
ES5中的顶层变量属性和全局变量挂钩，被认为是JS最大的败笔之一，
ES6中let，class和const声明的变量不再与顶层对象的属性关联


### Promise
```
var promise = new Promise(function (resolve, reject) {
        console.log('resolve');
		//成功回调
        resolve();
		//失败回调
		reject();
    })
    promise.then(function () {
        console.log('success')
    }, function () {
        console.log('fail')
    })
    console.log('justgo')
```

### export default {}为模块指定默认输出
export default function () {
  console.log('foo');
}
其他模块加载该模块时，import命令可以为该匿名函数指定任意名字
import customName from './export-default';

### ES6支持方法简写
```javascript
var a{
  points:function(){
     return this;
  }
}
* ES6可以简写为
var a{
   points(){
     return this;
   }
}
```

### export,export default的异同
1.export在js中可以有多个，export default只能出现一个；
2.在一个文件或模块中，export、import可以有多个，export default仅有一个
3.通过export方式导出，在导入时要加{ }，export default则不需要
4.export能直接导出变量表达式，export default不行。

### ES6的箭头函数
ES6中的箭头函数写法更加简单，表达更加简洁，简化回调函数
```
 var func = v => v+1;
等于：
 var func = function(v){return v+1}

var result = [1,2,1,3,4].sort((a,b)=>a-b)
等于：
var result = [1,2,1,3,4].sort(function(a,b){return a-b})
```

### 数组去重
```
var arr = [1,1,2,3,4,5,66,5,6];
console.log(new Set(arr))
//此时console出来的是一个对象，
console.log(array.from(new Set(arr)))
//此时console出来的是去重之后的数组

