---
title: JS常见问题
date: 2016-12-26 13:37:16
tags:
categories: js
---

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
