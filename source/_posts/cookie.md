---
title: cookie
date: 2016-09-14 18:07:27
tags:
categories: JS
---

### cookie
```javascript
//设置cookie  
//name是cookie中的名，value是对应的值，iTime是多久过期（单位为天）  
function setCookie(name,value,iTime){  
    var oDate = new Date();  
    //设置cookie过期时间  
    oDate.setDate(oDate.getDate()+iTime);  
    document.cookie = name+'='+value+';expires='+oDate.toGMTString();  
}  
//获取cookie  
function getCookie(name){  
    //cookie中的数据都是以分号加空格区分开  
    var arr = document.cookie.split("; ");  
    for(var i=0; i<arr.length; i++){  
        if(arr[i].split("=")[0] == name){  
            return arr[i].split("=")[1];  
        }  
    }  
    //未找到对应的cookie则返回空字符串  
    return '';  
}  
//删除cookie  
function removeCookie(name){  
    //调用setCookie方法，把时间设置为-1  
    setCookie(name,1,-1);  
}
```