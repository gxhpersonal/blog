---
title: ES6
date: 2017-02-06 09:34:53
tags:
categories: js
---

### let
let和var类似，let必须先声明后使用，否则会报错；
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
### Promise
