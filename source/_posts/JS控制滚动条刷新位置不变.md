---
title: JS控制滚动条刷新位置不变
date: 2016-12-01 10:40:18
tags:
categories: js
---
有时候，在网页中点击了页面中的按钮或是刷新了页面后，页面滚动条又 会回到顶部，想看后面的记录就又要拖动滚动条，或者要按翻页键，非常不方便，想在提交页面或者在页面刷新的时候仍然保持滚动条的位置不变，最好的办法就是 在JS中用cookie记录下当前滚动条的位置，然后刷新时读取cookie就可以实现这个功能了。
代码如下：
```js
  <script type="text/javascript">
    function  Trim(strValue)     
    {     
      return   strValue.replace(/^s*|s*$/g,"");     
    }
    
    function SetCookie(sName,sValue)     
    {     
      document.cookie = sName + "=" + escape(sValue);     
    }   
    
    function GetCookie(sName)     
    {     
     var aCookie = document.cookie.split(";");     
     for(var　i=0;　i　< aCookie.length;　i++)     
     {     
       var aCrumb = aCookie[i].split("=");     
       if(sName　== Trim(aCrumb[0]))     
       {     
         return unescape(aCrumb[1]);     
       }     
     }     
     
     　　return null;     
   } 
   
   function scrollback()     
   {     
     if(GetCookie("scroll")!=null){document.body.scrollTop=GetCookie("scroll")}     
   }     
</script>
```
然后在html页面中设置<body id=body   onscroll=SetCookie("scroll",body.scrollTop);   onload="scrollback();">就可以在刷新或提交后滚动条的位置保持不变了。

上面的是通用的解决方法，在.net中还可以用<pages maintainScrollPositionOnPostBack="true">这个配置，更容易
