---
title: JS字符串控制
date: 2016-11-30 18:27:25
tags: JS
categories: JS
---
### .split()方法
> stringObject.split(separator,howm)
> separator: 必需，字符串或正则表达式，以这个字符对应要操作的字符串中的字符分割
> howm: 可选，返回数组的最大长度
> 注 以多个字符分割字符串 正则搞定：
> stringObject.split(/[-,./]/)

### .splice()方法
> array.splice(index,howmany,item1,.....,itemX);
> 参数1必需。整数，规定添加/删除项目的位置，使用负数可从数组结尾处规定位置。，
> 参数2必需。要删除的项目数量。如果设置为 0，则不会删除项目。，
> 参数3可选。向数组添加的新项目。

### .slice()方法   可从  已有的数组中返回选定的元素 
>包含起始位置，不包含结束位置

### .reverse()方法
> arrayPbject.reverse()
> 用于颠倒数组中元素顺序

### .join()方法  //把数组中的所有元素放入一个字符串
> arrayObject.join(separator)
> 参数用来指定要使用的分隔符，默认为逗号

### .push(),.pop(),.unshift(),.shift()方法
.push()方法在当前数组末尾添加新元素，返回值为新数组长度；
.pop()方法删除当前数组最后一个元素并返回这个元素；
.unshift()方法向数组开始位置添加元素；
.shift() 方法用于把数组的第一个元素从其中删除，并返回第一个元素的值

### toLocaleString()方法
toLocaleString()除了可以数组转字符串，如：[1,2,3,4].toLocaleString() //'1,2,3,4'
还可以：
数字(不能是字符串)转为千分位格式 let num = 123456789;num..toLocaleString('en-US') //123,456,789

### flat() 方法
> 会按照一个可指定的深度(默认值为1)递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回
* 大白话就是，flat()可以把一个多层嵌套的数组转换成一个只有一层的数组
* 例子：
```js
let arr = [1,2,[3,4,5,[6,7,8]]];
arr.flat(2)
// [1,2,3,4,5,6,7,8]
arr.flat(1)
// [1,2,3,4,5,[6,7,8]]
arr.flat()
// [1,2,3,4,5,[6,7,8]]
```

### 数组去重
1.利用空数组+空对象push法：
```js
function removeArrSame1(arr){
	var tmp = {};
	var newA = [];
	for(var i=0;i<arr.length;i++){
 		if(!tmp[arr[i]]){
			tmp[arr[i]] = 1;
			newA.push(arr[i]);
	}
 }	
 return newA;
}
```
2.new一个新数组push方法：
```js
function removeArrSame2(arr){
	var newA = [];
	for(var i=0;i<arr.length;i++){
		if(newA.indexOf(arr[i]) === -1){
			newA.push(arr[i])
		}
	}
	return newA;
}
```

### .substr()方法
string.substr(star,length)
第一个参数要抽取的字串的起始下标，如果是负数，倒数，
第二个参数表示字串字符数

### string.substring()方法
返回介于两个指定下标之间的字符，取第一个参数，不取第二个参数，第二个参数不填，则返回到结尾；

### 字符串转换数字类型方法
1.parseInt(),parseFloat()
js提供了parseInt()和parseFloat()两个转换函数。前者把值转换成整数，后者把值转换成浮点数。只有对String类型调用这些方法，这两个函数才能正确运行；对其他类型返回的都是NaN(Not a Number)。

2.Number()
Number(value)——把给定的值转换成数字（可以是整数或浮点数）；

### 字符串大小写互转

##### 转换成小写
toLowerCase 方法
返回一个字符串，该字符串中的字母被转换为小写字母。

> strVariable.toLowerCase()
> "String Literal".toLowerCase()

说明
toLowerCase 方法对非字母字符不会产生影响。

> var strVariable = "This is a STRING object";
> strVariable = strVariable.toLowerCase();

值为：

> this is a string object

##### 转换成大写
toUpperCase 方法
返回一个字符串，该字符串中的所有字母都被转化为大写字母。

> strVariable.toUpperCase()
> "String Literal".toUpperCase()

说明
toUpperCase 方法对非字母字符不会产生影响。

> var strVariable = "This is a STRING object";
> strVariable = strVariable.toUpperCase();

值为：

> THIS IS A STRING OBJECT


### 输出图形字符

```javascript
 for(var i=1000;i<10000;i++)
{
  console.log(String.fromCharCode(i))
}
```

### 五种方法去除字符串最后的逗号
```html
<script>
window.onload=function() {
    var obj = {name: "xxx", age: 30, sex: "female"};//定义一个object对象
    var str = ''//定义一个空字符用来接收对象里的key或者value
    for(var item in obj) {//遍历item变量里的对象的属性和元素，
        str += obj[item] + ","//将obj对象的值遍历出来，并且追加到str字符中。
        //str += item + ","//将obj对象的属性遍历出来，并且追加到str字符中。
    }
    //第一种方法、将字符串中最后一个元素","逗号去掉，
    str = str.substring(0, str.lastIndexOf(','));

    //第二种方法、将字符串中最后一个元素","逗号去掉，
    str = (str.substring(str.length - 1) == ',') ? str.substring(0, str.length - 1) : str;

    //第三种方法、将字符串中最后一个元素","逗号去掉，
    var str=str.substring(0,str.length-1);//3、将字符串中最后一个元素","逗号去掉，

    //第四种方法、将字符串中最后一个元素","逗号去掉，
    var reg=/,$/gi;
    str=str.replace(reg,"");


    console.log(str)
}
</script>
//最后一种用css解决
<span ng-repeat="v in carNaP.sflb">{{v.Label}}<i>、</i></span>
<style>
span:last-child{
   i{
     display:none;
    }
}
</style>
```
