---
title: 前端与原生交互
date: 2018-01-22 14:25:23
tags: Native
categories: Native
---

### iOS中时间格式
ios不支持时间格式如：new Date(2018-01-24)格式转换,会抛出NaN,所以要转换成`/`格式的，如：
```
let date = 2018-02-12;
let newDate = date.replace(/-/g,'/');
let returnDate = new Date(newDate)
```

### iOS11.XX以上的iPhone注意
1.如果页面中有弹窗有input标签的，此弹窗（包括所有input的父级元素）不能用fixed布局，当弹窗被键盘顶上去，会导致input光标错乱，而且弹窗上面的按钮无法点击；
解决办法：弹层改为position：absolute，其他所有元素都隐藏，相当于页面只留一个弹层，不再让弹层脱离流

### iOS环境css :active伪元素不起作用
```
document.body.addEventListener('touchstart', function () { })
```
如上，要给body或者点击的元素加touchstart事件，来触发:active伪元素

### IOS支持3D touch的手机如果页面有a标签长按会触发3D touch并且跳转浏览器

### 与原生交互方法
```
imgesPreview: function (data) {
    if (Basic.isAppVersionAbove && Basic.isAppVersionAbove("2.4.7")) {
      //IOS
      if (
        window.webkit &&
        window.webkit.messageHandlers &&
        window.webkit.messageHandlers.imgesPreview
      ) {
		//如果传入的data为空，ios要传一个空字符串""
        window.webkit.messageHandlers.imgesPreview.postMessage(JSON.stringify(data));
        return true;
      }
      //Android
      if (window.AndroidCall && window.AndroidCall.imgesPreview) {
        window.AndroidCall.imgesPreview(JSON.stringify(data));
        return true;
      }
    }
    return false;
  }

//调用：
Webview.imgesPreview(dataObj);
```

### ios下滚动背景body导致弹层中滚动条失效
```
//其中fixed为一个class给body设置fixed样式
fixedHelper: (function(bodyCls) {
   var scrollTop;
            return {
                afterOpen: function() {
					//document.scrollingElement 返回滚动文档的 Element 对象的引用
                    scrollTop = document.scrollingElement.scrollTop;
                    document.body.classList.add(bodyCls);
                    document.body.style.top = -scrollTop + 'px';
                },
                beforeClose: function() {
                    document.body.classList.remove(bodyCls);
                    // scrollTop lost after set position:fixed, restore it back.
                    document.scrollingElement.scrollTop = scrollTop;
                }
            };         
})('fixed')
```

### 拦截Android自带的物理返回键
```
//拦截安卓回退按钮
    history.pushState(null, null, location.href);
    window.addEventListener("popstate", function(event) {
      history.pushState(null, null, location.href);
      //此处加入回退时你要执行的代码
    });
```