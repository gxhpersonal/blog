---
title: js黑科技
date: 2019-05-02 10:41:23
tags: JS
categories: JS
---

## 主要记录了js中不常用的一些技巧和逻辑

### 1.js中 `&&` 和 `&` ，`||` 和 `|` 的区别：
##### && 
```js
console.log(1 && 2); // 2
```
JS中的逻辑运算符 `&&` 与，左边为真才走右边，左边为假只走左边。

##### &
```js
console.log(1 & 2); // 0
console.log(0 & 0)  // 0
console.log(1 & 123)  // 1
```
&是表示位的与运算，把左右两边的数字转化为二进制，然后每一位分别进行比较，如果相等就为1，不相等即为0。
同时&具有强制转换的功能，把false,true转换成了0和1进行比较。而0又表示false的意思，所以带有false的都挂掉了，偶尔也可以当做逻辑与来使用。
1转换为二进制为1,2转换为二进制为10，  所以1&2实际是0001&0010,没有相等的部分，最终结果就是0了。

### 2.获取url参数最简写法
```js
q={};location.search.replace(/([^?&=]+)=([^&]+)/g,(_,k,v)=>q[k]=v);q;
```

### 3.返回一个键盘？
```js
(_=>[..."`1234567890-=~~QWERTYUIOP[]\\~ASDFGHJKL;'~~ZXCVBNM,./~"].map(x=>(o+=`/${b='_'.repeat(w=x<y?2:' 667699'[x=["BS","TAB","CAPS","ENTER"][p++]||'SHIFT',p])}\\|`,m+=y+(x+'    ').slice(0,w)+y+y,n+=y+b+y+y,l+=' __'+b)[73]&&(k.push(l,m,n,o),l='',m=n=o=y),m=n=o=y='|',p=l=k=[])&&k.join`
`)()
```

### js原生 警告，确认，信息输入弹窗
alert confirm prompt


### console玩法
```js
console.groupCollapsed('这里是外面');
  console.log('这里是里面');
console.groupEnd();

  //小区别：
  - console.group 默认是展开状态
  - console.groupCollapsed 默认是收起状态
```