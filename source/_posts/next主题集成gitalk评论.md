---
title: next主题集成gitalk评论
date: 2019-03-11 15:34
tags: blog
categories: blog
---

## 记录一下hexo博客next主题集成gitalk评论插件的步骤
> 本文参考[https://asdfv1929.github.io/2018/01/20/gitalk/](https://asdfv1929.github.io/2018/01/20/gitalk/)
> gitalk：一个基于 Github Issue 和 Preact 开发的评论插件
> 官网：[https://gitalk.github.io/](https://gitalk.github.io/)

### 1.在自己的GitHub中注册一个开放授权的gitalk应用
![](http://www.guoxh.com/blog/img/blog/register.png)
> 参数说明：
  Application name： # 应用名称，随意
  Homepage URL： # 网站URL，如https://gxhpersonal.github.io
  Application description # 描述，随意
  Authorization callback URL：# 网站URL，https://gxhpersonal.github.io

### 2.点击注册后，页面跳转如下，其中Client ID和Client Secret在后面的配置中需要用到，到时复制粘贴即可：
![](http://www.guoxh.com/blog/img/blog/register2.png)

### 3.在next主题文件下新建/layout/_third-party/comments/gitalk.swig文件，并添加内容：
```
  {% if page.comments && theme.gitalk.enable %}
  <link rel="stylesheet" href="https://unpkg.com/gitalk/dist/gitalk.css">
  <script src="https://unpkg.com/gitalk/dist/gitalk.min.js"></script>
   <script type="text/javascript">
        var gitalk = new Gitalk({
          clientID: '{{ theme.gitalk.ClientID }}',
          clientSecret: '{{ theme.gitalk.ClientSecret }}',
          repo: '{{ theme.gitalk.repo }}',
          owner: '{{ theme.gitalk.githubID }}',
          admin: ['{{ theme.gitalk.adminUser }}'],
          id: location.pathname,
          distractionFreeMode: '{{ theme.gitalk.distractionFreeMode }}'
        })
        gitalk.render('gitalk-container')           
       </script>
{% endif %}
```
> 其中的配置项如clientID，clientSecret等对应为主题配置文件_config.yml中的配置项

### 4.修改/layout/_partials/comments.swig，添加内容如下，与前面的elseif同一级别上：
```
{% elseif theme.gitalk.enable %}
 <div id="gitalk-container"></div>

```
![](http://www.guoxh.com/blog/img/blog/register3.png)

### 5.修改layout/_third-party/comments/index.swig，在最后一行添加内容：
```
{% include 'gitalk.swig' %}
```