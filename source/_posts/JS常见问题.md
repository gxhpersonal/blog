---
title: JS常见问题
date: 2016-12-26 13:37:16
tags:
categories: JS
---

### Ajax跨域问题解决
[http://www.cnblogs.com/pandang/p/5341250.html]()

### JS判断浏览器种类IE，FF，Opera,Safari,chrome...
[http://www.jb51.net/article/50464.htm]()

//基本原理就是根据浏览器的userAgent判断
var userAgent = navigator.userAgent; 
//例如：
if (userAgent.indexOf("Safari") > -1) {
        return "Safari";
} //判断是否Safari浏览器

### 警告，确认，信息输入弹窗
alert confirm prompt

### encodeURIComponent函数
1.encodeURIComponent() 函数可把字符串作为 URI 组件进行编码。
* 请注意 encodeURIComponent() 函数 与 encodeURI() 函数的区别之处，前者假定它的参数是 URI 的一部分（比如        协议、主机名、路径或查询字符串）。因此 encodeURIComponent() 函数将转义用于分隔 URI 各个部分的标点符号。

###超出指定字符数显示省略号
console.log($(".friend_detail p").text().length)
    var str = $(".friend_detail p").text();
    var s = str;
    if (str.length > 6) {
        s = str.substring(0,6) + "...";
    }
    $(".friend_detail p").text(s);

### 移动端input被输入法挡住
scrollIntoView(alignWithTop)

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
```
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
```
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

### 通过身份证号得到生日，性别，年龄
```
<script>
function IdCard(UUserCard,num){
   if(num==1){
       //获取出生日期
       birth=UUserCard.substring(6, 10) + "-" + UUserCard.substring(10, 12) + "-" + UUserCard.substring(12, 14);
    return birth;
   }
   if(num==2){
       //获取性别
       if (parseInt(UUserCard.substr(16, 1)) % 2 == 1) {
           //男
     return "男";
       } else {
           //女
     return "女";
       }
   }
   if(num==3){
        //获取年龄
        var myDate = new Date();
        var month = myDate.getMonth() + 1;
        var day = myDate.getDate();
        var age = myDate.getFullYear() - UUserCard.substring(6, 10) - 1;
        if (UUserCard.substring(10, 12) < month || UUserCard.substring(10, 12) == month && UUserCard.substring(12, 14) <= day) {
            age++;
        }
  return age;
 }
}
alert (IdCard('142223198503226111',3));
</script>
```

###APP环境登录判断
        //登录拦截主方法，url:登录成功后跳转链接
        CheckLoginAndGoNext: function (url) {
            if (!url) {
                url = window.location.href;
            }
            window.hzch5.CheckLogin(url);
        },

        //登录拦截 第一步：check登录状态
        CheckLogin: function (url) {
            if (!url) {
                url = window.location.href;
            }
            $.ajax({
                url: "http://"+window.location.host+"/Account/UserLoginStatus", type: "POST", dataType: "json", success: function (data) {
                    if (data.success) {
                        window.location = url;
                    }
                    else {
                        window.hzch5.ToLoginPage(url);
                    }
                }
            });
        },
          //登录拦截 第二步：未登录跳转登录界面（H5/APP)
        ToLoginPage: function (url) {
            if (!url) {
                url = window.location.href;
            }
            if (navigator.userAgent.indexOf("hzc-ios") > -1 || navigator.userAgent.indexOf("hzc-android") > -1) {
                var cver = window.hzch5.getCookie("cver"); //app版本号
                var token = window.hzch5.getCookie("token");
                if (cver) {//2.4版本
                    var token = window.hzch5.getCookie("token");
                    if (token) {
                        //根据token同步APP登录状态
                        window.hzch5.checkLoginByToken(token, url);
                        return false;
                    }
                    document.location = "WINDOW:HZC:LOGIN";                   
                }
                else {//小于2.4版本
                document.location = "http://" + window.location.host + "/Account/userLogin?returnurl=" + encodeURIComponent(url);
                }
            }
            else {
                document.location = "http://" + window.location.host + "/Account/userLogin?returnurl=" + encodeURIComponent(url);
            }
        },
          //登录拦截 第三步  APP登录状态同步
        checkLoginByToken: function (token, url) {
            if (!url) {
                url = window.location.href;
            }
            if (!token) {
                token = window.hzch5.getCookie("token");
            }
            $.ajax({
                url: "http://" + window.location.host + "/Account/UserLoginAuto", type: "POST", dataType: "json", data: { token: token }, success: function (data) {
                    if (data.success) {
                        document.location = "http://" + window.location.host + "/Account/userLogin?returnurl=" + encodeURIComponent(url);
                    }
                    else {                      
                        document.location = "WINDOW:HZC:LOGIN";
                    }
                }
            });
        },
        //登录拦截 结束
