---
title: JS常见问题
date: 2016-12-26 13:37:16
tags: JS
categories: JS
---

### do not track (禁止跟踪)
[https://www.zhangxinxu.com/wordpress/2018/07/navigator-do-not-track-api/]()

### Ajax跨域问题解决
[http://www.cnblogs.com/pandang/p/5341250.html]()

### JS判断浏览器种类IE，FF，Opera,Safari,chrome...

//基本原理就是根据浏览器的userAgent判断
var userAgent = navigator.userAgent; 
//例如：
if (userAgent.indexOf("Safari") > -1) {
        return "Safari";
} //判断是否Safari浏览器

### encodeURIComponent()和decodeURIComponent()函数
1.encodeURIComponent() 函数可把字符串作为 URI 组件进行编码。
* 请注意 encodeURIComponent() 函数 与 encodeURI() 函数的区别之处，前者假定它的参数是 URI 的一部分（比如        协议、主机名、路径或查询字符串）。因此 encodeURIComponent() 函数将转义用于分隔 URI 各个部分的标点符号。
2.decodeURIComponent() 对 encodeURIComponent() 函数编码的 URI 进行解码

### escape() encodeURI() encodeURIComponent()
escape() 方法：

采用ISO Latin字符集对指定的字符串进行编码。所有的空格符、标点符号、特殊字符以及其他非ASCII字符都将被转化成%xx格式的字符编码（xx等于该字符在字符集表里面的编码的16进制数字）。比如，空格符对应的编码是%20。unescape方法与此相反。不会被此方法编码的字符： @ * / +
* 对应的解码方法：unescape()

encodeURI() 方法：
把URI字符串采用UTF-8编码格式转化成escape格式的字符串。不会被此方法编码的字符：! @ # $& * ( ) = : / ; ? + '
* 对应的解码方法：decodeURI()

encodeURIComponent() 方法：

把URI字符串采用UTF-8编码格式转化成escape格式的字符串。与encodeURI()相比，这个方法将对更多的字符进行编码，比如 / 等字符。所以如果字符串里面包含了URI的几个部分的话，不能用这个方法来进行编码，否则 / 字符被编码之后URL将显示错误。不会被此方法编码的字符：! * ( ) 
* 对应的解码方法：decodeURIComponent()

### JS监听动画完成事件
tt.addEventListener("webkitAnimationEnd", function(){ //动画结束时事件 
this.className = this.className.replace('change', ' '); 
}, false); 

### 元素scrollTop属性
> 只有元素设置了overflow样式，且不为hidden时才会生效，jquery对象要加下标[index]，否则找不到此属性；

### JS处理Range
>所谓"Range"，是指HTML文档中任意一段内容。一个Range的起始点和结束点位置任意，甚至起始点和结束点可以是一样的（也就是空Range）。最常见的Range是用户文本选择范围(user text selection)。当用户选择了页面上的某一段文字后，你就可以把这个选择转为Range。当然，你也可以直接用程序定义Range
>举几个常用的处理Range的方法：
1.获取用户选中的文本：
```JS
var oBtn = document.getElementById("button");
oBtn.onclick = function() {
    var userSelection, text;
    if (window.getSelection) { 
        //现代浏览器
        userSelection = window.getSelection();
    } else if (document.selection) { 
        //IE浏览器 考虑到Opera，应该放在后面
        userSelection = document.selection.createRange();
    }
    if (!(text = userSelection.text)) {
        text = userSelection;
    }
    alert(text);
};
```
2.点击表单内容全选：
```JS
function SelectText(name) {
	if(obj.createTextRange){//IE浏览器
    var range = obj.createTextRange();              
    range.moveEnd("character",结束序号);
    range.moveStart("character", 起始序号);
    range.select();
}else{//非IE浏览器
    obj.setSelectionRange(起始序号, 结束序号);
    obj.focus();
}
}
```
其他详见张鑫旭博客：
[http://www.zhangxinxu.com/wordpress/2011/04/js-range-html%E6%96%87%E6%A1%A3%E6%96%87%E5%AD%97%E5%86%85%E5%AE%B9%E9%80%89%E4%B8%AD%E3%80%81%E5%BA%93%E5%8F%8A%E5%BA%94%E7%94%A8%E4%BB%8B%E7%BB%8D/]()

### clipboard.js实现移动端复制功能(！！！重中之重：iOS下要给点击的元素加onclick=""或者cursor：pointer，总之就是让iOS可以识别到这个元素可点击)
```HTML
<p class="J-browserLink" style="cursor: pointer;">点击复制1</p>
<script src="https://cdn.bootcss.com/clipboard.js/1.6.0/clipboard.js"></script>
<script>
var clipboard1 = new Clipboard(".J-browserLink", {
	text: function () {
		return $(".J-browserLink").html()
	}
})
clipboard1.on("success", function () {
	alert('复制成功');
})
</script>
```

### window.location
返回当前 URL 的路径名：
console.log(location.pathname);

加载一个新的文档：
window.location.assign("http://www.w3school.com.cn")

  ● location.hostname 返回 web 主机的域名
  ● location.pathname 返回当前页面的路径和文件名
  ● location.port 返回 web 主机的端口 （80 或 443）
  ● location.protocol 返回所使用的 web 协议（http:// 或 https://）

### app内返回不刷新导致订单状态没有更新解决，强制刷新
```js
JumpUrlForReturn: function (url) {
    //定义页面展示时触发事件
    window.onpageshow = function (event) {
        //从缓存中读取页面时触发
        if (event.persisted && WebStorage.forceRefresh) {
            WebStorage.removeItem("forceRefresh")
            window.location.reload(true);//强制刷新                  
        }
    }

    //兼容不支持onpageshow: event.persisted 
    var refresh = hzch5.getParameterByName("refresh");
    var curl = window.location.href;
    if (curl.indexOf("?") > -1) {
        if (refresh && refresh != "" && curl.indexOf("refresh") > -1) {
            curl = curl.replace("refresh=" + refresh, "refresh=" + Math.random());
        }
        else {
            curl += "&refresh=" + Math.random();
        }
    }
    else {
        curl += "?refresh=" + Math.random();
    }
    window.history.replaceState("", window.document.title, curl);
    //End 兼容不支持onpageshow: event.persisted 

    url && (window.location.href = url);
}
```

### 通过身份证号得到生日，性别，年龄
```html
<script>
//获取出生日期，7位到15位为出生年月
birth=UUserCard.substring(6, 10) + "-" + UUserCard.substring(10, 12) + "-" + UUserCard.substring(12, 14);
//获取性别（其原理就是取身份证第17位除以2，余数1是男，否则是女）
parseInt(UUserCard.substr(16, 1)) % 2 == 1
//获取年龄（年龄=当前年-身份证7至10位数字，如果月比当前月小或者月与当前月相等&&日比当前日小于等于，则年龄+1）

</script>
```

### 时间格式转换
>Date 对象方法
每个get方法都有对应的一个set方法，如：
getFullYear() 对应 setFullYear()
getMonth() 对应 setMonth()
getTime() 对应 setTime() //返回 1970 年 1 月 1 日至今的毫秒数  和  	以毫秒设置 Date 对象。
> Date.parse()和getTime()的区别：
> getTime()方法是把一个date对象转成毫秒；parse()方法是把一个时间格式的字符串转换成毫秒。

### 多个同类型元素控制别的同类型的元素
举个栗子：
通过点击当前tab要控制侧边栏的导航tab，因为两个tab中的同级元素很多，所以可以根据他们的id来分别控制；
给点击的tab元素设置不同的id,`如：current1`，给被控制的元素设置id，`如：nav-current1`,与点击的元素id对应就好，然后就可以根据`nav-xxx`
来对应元素进行操作

### JS实现滚动显示，停止滚动隐藏
```js
var timer;
window.onscroll = function () {
   $('.div').hide()
   if (timer){
      clearTimeout(timer)
   }
   timer = setTimeout(function () {
      $('.div').show()
   }, 1000)
}
```

### JS兼容H5手机实现弹窗出现禁止背景滚动
既然我们要阻止页面滚动，那么何不将其固定在视窗（即position: fixed），这样它就无法滚动了，当蒙层关闭时再释放。
当然还有一些细节要考虑，将页面固定视窗后，内容会回头最顶端，这里我们需要记录一下，同步top值。
```js
let bodyEl = document.body
let top = 0

function stopBodyScroll (isFixed) {
  if (isFixed) {
    top = window.scrollY

    bodyEl.style.position = 'fixed'
    bodyEl.style.top = -top + 'px'
  } else {
    bodyEl.style.position = ''
    bodyEl.style.top = ''

    window.scrollTo(0, top) // 回到原先的top
  }
}
```

### js获取链接中的参数
```js
//name为参数名，queryString为链接
getParameterByName: function (name,queryString) {
  name = name.replace(/[\[]/, "\\[").replace(/[\]]/, "\\]");
  if (!queryString) {
      queryString = location.search;
  }
  var regex = new RegExp("[\\?&]" + name + "=([^&#]*)"),
  results = regex.exec(queryString);
  return results == null ? "" : decodeURIComponent(results[1].replace(/\+/g, " "));
}
```

### canvas图片跨域
因为生成图片时，生成图片路径和已有图片路径不同，可以把图片转为base64
canvas.toDataURL(type, encoderOptions);

### 常用正则
1.邮箱：/^\w+((-\w+)|(\.\w+))*\@[A-Za-z0-9]+((\.|-)[A-Za-z0-9]+)*\.[A-Za-z0-9]+$/
2.手机号：/^1[3|4|5|6|7|8|9][0-9]{9}$/
3.价格匹配：/(?!^0*(\.0{1,2})?$)^\d{1,13}(\.\d{1,2})?$/  <!-- 如果要精确到小数点任意位数，把最后面{1,2}中的2改为指定位数 -->

### 对象数组合并相同ID的对象并计算出数量
```js
//使用reduce累加器特性过滤相同id的项并计算数量
let sameArr = [{id:111,name:"张三"},{id:222,name:"李四"},{id:111,name:"张三"}]
let arr = sameArr;
arr = arr.reduce((obj, item) => {
let find = obj.find(i => i.id === item.id)
let _d = {
    ...item,
    goods_count: 1,
    isShow: false
}
if (find) {
    find.goods_count++;
    find.isShow = true;
} else {
    _d.isShow = true;
}
obj.push(_d)
return obj
}, [])
// console.log(arr)
sameArr = arr;
```

### HTML5 - 使用地理定位
```js
function getLocation() {
	if (navigator.geolocation) {
		navigator.geolocation.getCurrentPosition(showPosition, showError);
	} else {
		console.log("此浏览器不支持地理位置");
	}
}
getLocation();
function showPosition(position) {
	console.log("Latitude: " + position.coords.latitude + " Longitude: " + position.coords.longitude);
}
function showError(error) {
	switch (error.code) {
		case error.PERMISSION_DENIED:
			console.log("用户拒绝了地理位置的请求");
			break;
		case error.POSITION_UNAVAILABLE:
			console.log("位置信息不可用");
			break;
		case error.TIMEOUT:
			console.log("获取用户位置的请求超时");
			break;
		case error.UNKNOWN_ERROR:
			console.log("发生未知错误");
			break;
	}
}
```

### Selection.collapse() 方法可以收起当前选区到一个点。文档不会发生改变。如果选区的内容是可编辑的并且焦点落在上面，则光标会在该处闪烁
要获取用于检查或修改的 Selection 对象，请调用 window.getSelection()
window.getSelection().collapse(parentNode, offset);
参数
parentNode
光标落在的目标节点。
offset 可选
落在节点的偏移量。


### 防抖：延时执行函数，如果存在定时器，则清空定时器，重新设置定时器延时执行；节流：先立刻执行一次函数，进入延时，直到这个函数执行完成，才能再次执行函数