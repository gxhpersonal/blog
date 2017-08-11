---
title: JS字符串控制
date: 2016-11-30 18:27:25
tags:
categories: JS
---
### .split()方法
> stringObject.split(separator,howm)
> separator: 必需，字符串或正则表达式，以这个字符对应要操作的字符串中的字符分割
> howm: 可选，返回数组的最大长度

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

###判断电话号码
```javascript
validateMobile: function (str) {
            var reg = /^1[3-8]\d{9}$/;
            return reg.test(str);
        }
//邮箱验证
var reg = /^\w+((-\w+)|(\.\w+))*\@[A-Za-z0-9]+((\.|-)[A-Za-z0-9]+)*\.[A-Za-z0-9]+$/;
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

### 数组去重
1.利用空数组+空对象push法：
```
function removeArrSame(arr){
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

### 字符串大小写互转
toLocaleUpperCase 方法
返回一个字符串，其中所有的字母字符都被转换为大写，同时适应宿主环境的当前区域设置。

> stringVar.tolocaleUpperCase()

必选的 stringVar 引用是一个 String 对象，值或文字。

说明
toLocaleUpperCase 方法转换字符串中的字符，同时适应宿主环境的当前区域设置。在大多数情况下，其结果与利用 toUpperCase 方法所得到的结果是一样的。然而，如果语言规则与常规的 Unicode 大小写映射方式冲突，那么结果就会不同。


toLocaleLowerCase 方法
返回一个字符串，其中所有的字母字符都被转换为小写，同时考虑到宿主环境的当前区域设置。

> stringVar.tolocaleLowerCase()

必选的 stringVar 引用是一个 String 对象，值或文字。

说明
toLocaleLowerCase 方法转换字符串中的字符，同时适应宿主环境的当前区域设置。在大多数情况下，其结果与利用 toLowerCase 方法所得到的结果是一样的。然而，如果语言规则与常规的 Unicode 大小写映射方式冲突，那么结果就会不同。

//转换成小写
toLowerCase 方法
返回一个字符串，该字符串中的字母被转换为小写字母。

> strVariable.toLowerCase()
> "String Literal".toLowerCase()

说明
toLowerCase 方法对非字母字符不会产生影响。

下面的示例演示了 of the toLowerCase 方法的效果：

> var strVariable = "This is a STRING object";
> strVariable = strVariable.toLowerCase();
在执行上一条语句后 strVariable 的值为：

> this is a string object

//转换成大写
toUpperCase 方法
返回一个字符串，该字符串中的所有字母都被转化为大写字母。

> strVariable.toUpperCase()
> "String Literal".toUpperCase()

说明
toUpperCase 方法对非字母字符不会产生影响。

示例
下面的示例演示了 toUpperCase 方法的效果：

> var strVariable = "This is a STRING object";
> strVariable = strVariable.toUpperCase();
> 
在执行上一条语句后 strVariable 的值为：

> THIS IS A STRING OBJECT

### 输出图形字符

```javascript
 for(var i=1000;i<10000;i++)
{
  console.log(String.fromCharCode(i))
}
```

