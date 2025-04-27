---
title: uni-app
date: 2023-10-17 14:52:19
tags: uni-app
categories: uni-app
---

### 条件编译快捷命令
选中要条件编译的代码块，`ctrl + alt + /`即可生成正确注释
```js
// #ifdef APP
uni.hideNavigationBarLoading();
// #endif
```

### 滚动一屏，sticky失效
需要把 sticky元素放在 一个 view 中，不能放在 template 中  