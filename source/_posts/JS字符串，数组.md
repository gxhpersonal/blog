---
title: JS字符串控制
date: 2016-11-30 18:27:25
tags:
categories: js
---
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
### .split()方法
> stringObject.split(separator,howm)
> separator: 必需，字符串或正则表达式，以这个字符对应要操作的字符串中的字符分割
> howm: 可选，返回数组的最大长度

### .reverse()方法
> arrayPbject.reverse()
> 用于颠倒数组中元素顺序

### .join()方法
> arrayObject.join(separator)
> 参数用来指定要使用的分隔符，默认为逗号

###判断电话号码
```javascript
validateMobile: function (str) {
            var reg = /^1[3-8]\d{9}$/;
            return reg.test(str);
        }
```

### .substr()方法
string.substr(star,length)
第一个参数要抽取的字串的起始下标，如果是负数，倒数，
第二个参数表示字串字符数

### string.substring()方法
返回介于两个指定下标之间的字符，取第一个参数，不取第二个参数，第二个参数不填，则返回到结尾；