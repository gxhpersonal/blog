---
title: JS常见问题
date: 2016-12-26 13:37:16
tags:
categories: js
---

### Ajax跨域问题解决
[http://www.cnblogs.com/pandang/p/5341250.html]()


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
