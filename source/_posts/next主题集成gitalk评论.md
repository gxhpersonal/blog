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
          id: location.pathname        })
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

### 6.在主题配置文件next/_config.yml中添加如下内容：
```
gitalk:
  enable: true
  githubID: github帐号ID  # 例：gxhpersonal 
  repo: 仓库名称   # 例：gxhpersonal.github.io
  ClientID: Client ID #上面申请的授权应用id
  ClientSecret: Client Secret #上面申请的授权应用Secret
  adminUser: github帐号 #指定可初始化评论账户 例：gxhpersonal 
```

#### 以上
***
#### 以下为可能遇到的问题

### Error Not Found
检查自己的配置项字段是否正确，如：
1> repo：gxhpersonal.github.io后面不能加path路径
2> 主题配置文件next/_config.yml中的字段要和/layout/_third-party/comments/gitalk.swig文件中的引用字段对应

### Error: Validation Failed
由于 Github 限制 labal 长度不能超过 50引起的
具体解决是通过MD5加密ID来缩短labal长度
1> 创建`md5.min.js`文件在 \themes\next\source\js\src\md5.min.js[官网GitHub](https://github.com/blueimp/JavaScript-MD5/blob/master/js/md5.min.js)
2> 修改gitalk.swig文件
```
  {% if page.comments && theme.gitalk.enable %}
  <link rel="stylesheet" href="https://unpkg.com/gitalk/dist/gitalk.css">
  <script src="https://unpkg.com/gitalk/dist/gitalk.min.js"></script>

  ``引入文件 <script src="/js/src/md5.min.js"></script>``

   <script type="text/javascript">
        var gitalk = new Gitalk({
          clientID: '{{ theme.gitalk.ClientID }}',
          clientSecret: '{{ theme.gitalk.ClientSecret }}',
          repo: '{{ theme.gitalk.repo }}',
          owner: '{{ theme.gitalk.githubID }}',
          admin: ['{{ theme.gitalk.adminUser }}'],     
          
          ``删除配置     id: location.pathname,``
          ``添加配置     id: md5(location.pathname),``

          })
        gitalk.render('gitalk-container')           
       </script>
{% endif %}
```
