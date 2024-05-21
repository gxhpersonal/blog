---
title: ES6
date: 2017-02-06 09:34:53
tags: JS
categories: JS
---

### let  const
let和var类似，let必须先声明后使用，否则会报错，let不存在变量提升；
let声明变量，const声明常量；
```js
function block(){
  let n = 5;
  if(true){
    let n = 10;
  }
  console.log(n) //5
}
block()
```
上面代码说明let有块级作用域，不同块级相同变量不受影响；

### 顶层对象的属性和全局变量关系
ES5中的顶层变量属性和全局变量挂钩，被认为是JS最大的败笔之一，
ES6中let，class和const声明的变量不再与顶层对象的属性关联


### Promise(主要解决异步回调地狱的问题)
```js
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
> `Promise.prototype.then()`第一个函数参数为resolve执行函数，第二个函数参数为reject执行函数

### export default {}为模块指定默认输出
```js
export default function () {
  console.log('foo');
}
// 其他模块加载该模块时，import命令可以为该匿名函数指定任意名字
import customName from './export-default';
```

### ES6支持方法简写
```javascript
var a{
  points:function(){
     return this;
  }
}
// ES6可以简写为
var a{
   points(){
     return this;
   }
}
```

### export,export default的异同点
1.export在js中可以有多个，export default只能出现一个；
2.在一个文件或模块中，export、import可以有多个，export default仅有一个
3.通过export方式导出，在导入时要加{ }，export default则不需要
4.export能直接导出变量表达式，export default不行。

### ES6的箭头函数
ES6中的箭头函数写法更加简单，表达更加简洁，简化回调函数
```js
 var func = v => v+1;
 // 等于：
 var func = function(v){return v+1}

var result = [1,2,1,3,4].sort((a,b)=>a-b)
// 等于：
var result = [1,2,1,3,4].sort(function(a,b){return a-b})
```

### 数组去重
```js
var arr = [1,1,2,3,4,5,66,5,6];
console.log(new Set(arr))
//此时console出来的是一个对象，
console.log(array.from(new Set(arr)))
//此时console出来的是去重之后的数组
```

### 字符串拼接
```js
ES5:
var func = function(v){
	console.log('name '+v)
}
func('jone')

ES6:
// 又叫模板字符串
var func = v => console.log(`name ${v}`)
func('jone')
```

### ES6支持变量作为对象key，如：
```js
let a = 'aaa';
let b = 'bbb';
let c = {[a]:'this is key a',[b]:'this is key b'};
console.log(c)
```

### 生成器函数指定下一次调用 next() 时会生成什么 value
```js
function* greeter(){
  yeild "hello"
  yeild "how are you"
  yeild "good bye"
}
const greet = greeter();
console.log(greet.next().value);
// 'Hi'
console.log(greet.next().value);
// 'How are you?'
console.log(greet.next().value);
// 'Bye'
console.log(greet.next().value);
// undefined
// or
// 使用生成器生成无限个值：
function* idCreator() {
  let i = 0;
  while (true)
    yield i++;
}
const ids = idCreator();
console.log(ids.next().value);
// 0
console.log(ids.next().value);
// 1
console.log(ids.next().value);
// 2
// etc...
```

### Async/Await
在掌握了 promise 的用法后，你可能也会喜欢 async await，它只是一种基于 promise 的“语法糖”。在下面的示例中，我们创建了一个 async 函数，并 await greeter promise。
```ES6
const greeter = new Promise((res, rej) => {
  setTimeout(() => res('Hello world!'), 2000);
})
async function myFunc() {
  const greeting = await greeter;
  console.log(greeting);
}
myFunc();
// 'Hello world!'
// 可以看到，async创建的函数体内，加了await的函数，即使它是异步的，也会等他先执行完
```

### ES2020新增链判断运算符 及 Null/undefined判断运算符
1.`?.`链判断运算符
```js
//错误的写法 这样如果user及前面的值没取到会报错
let name = data.result.user.name;
//正确的写法
let name = (data && data.result && data.result.user && data.result.user.name) || '小明';
// 非常麻烦，所以ES2020引入 ?. 运算符
let name = data?.result?.user?.name || '小明';
// 上面例子中，只要有一层返回null或者undefined就不再往下取值，直接返回undefined
// 取一个数组指定下标值：
let arr = array?.[1]
// 引申示例：
if(arr && arr.length > 0){...}
if(arr && arr.length){...} //如果arr是数组，arr.length为0，按照隐式转换不会执行里面代码，所以不需要判断 > 0 
if(arr?.length){...}  //这就是链判断运算符的好处，节省代码，语义明确
```

2.`??`Null/undefined判断运算符

```js
const headerText = response.settings.headerText || 'Hello, world!';
const headerText = response.settings.headerText ?? 'Hello, world!';
```
上面两种例子的区别是`||`左边的值为false、空字符串、0都会生效，而`??`只有左边为undefined或者null才会取右边的默认值；

### ES13 (ECMAScript 2022) 新增以下功能：
##### 顶级await
从[ES2022](https://github.com/tc39/proposal-top-level-await)开始，允许在模块的顶层独立使用`await`命令，使得上面那行代码不会报错了。它的主要目的是使用`await`解决模块异步加载的问题，如下：
```js
const data = await fetch('https://api.example.com');
// or
const showBlackTheme = window.location.search.includes('theme=black')
if (showBlackTheme) {
  await import('/theme/black.js')
} else {
  await import('/theme/white.js')
}
```

##### Object.hasOwn
我们经常需要知道对象上是否存在某个属性。怎么做？

“in”或“obj.hasOwnProperty”是用于此目的的两种最常用的方法。

如果指定的属性位于指定的对象或其原型链中，则 in 运算符返回 true。
```js
const Person = function (age) {
  this.age = age
}

Person.prototype.name = 'fatfish'

const p1 = new Person(24)

console.log('age' in p1) // true 
console.log('name' in p1) // true
```
hasOwnProperty 方法返回一个布尔值，指示对象是否将指定属性作为其自己的属性（而不是继承它）。
```js
const Person = function (age) {
  this.age = age
}

Person.prototype.name = 'fatfish'

const p1 = new Person(24)

console.log(p1.hasOwnProperty('age')) // true 
console.log(p1.hasOwnProperty('name')) // fasle
```
以下情况使用`hasOwnProperty`会报错
```js
Object.create(null).hasOwnProperty('name')
// Uncaught TypeError: Object.create(...).hasOwnProperty is not a function
```
不用担心，我们可以使用“Object.hasOwn”来规避这两个问题，这比“obj.hasOwnProperty”方法更方便、更安全。
```js
let object = { age: 24 }

Object.hasOwn(object, 'age') // true

let object2 = Object.create({ age: 24 })

Object.hasOwn(object2, 'age') // false  The 'age' attribute exists on the prototype

let object3 = Object.create(null)

Object.hasOwn(object3, 'age') // false an object that does not inherit from "Object.prototype"
```

##### 数组`.at()`方法
> at 方法可以取正数或负数，这将决定它是从数组的头部还是尾部开始读取元素。
```js
const arr = [ 1, 2, 3, 4, 5 ]
const lastItem = array.at(-1) // 5
const firstItem = array.at(0) // 1
```

##### 私有槽位`#`及方法
使用`#`来实现真正安全的私有属性
```js
class Person {
  #money=1
  constructor (name) {
    this.name = name
  }
  get money () {
    return this.#money
  }
  set money (money) {
    this.#money = money
  }
  showMoney () {
    console.log(this.#money)
  }
}
const p1 = new Person('fatfish')
console.log(p1.money) // 1
// p1.#money = 2 // We cannot modify #money in this way
p1.money = 2
console.log(p1.money) // 2
console.log(p1.#money) // Uncaught SyntaxError: Private field '#money' must be declared in an enclosing class

```

##### 实例方法：toReversed()，toSorted()，toSpliced()，with()

##### 实例方法：group()，groupToMap() (只是提案，未能使用)

##### Math.trunc()