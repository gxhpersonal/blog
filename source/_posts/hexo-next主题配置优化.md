---
title: hexo next主题配置优化
date: 2016-11-22 13:44:16
tags:
categories: blog
---

### 技术迭代版本飞快，只有时刻保持学习和探索，才能跟上不掉队，框架更是如此，需要不断更新，不断修改文章记录，如果一成不变，那写文章将毫无意义

### NEXT主题官方文档，操作扩展都非常详细，有兴趣可以看一看 
[NEXT主题配置](http://theme-next.iissnan.com/theme-settings.html)   
[NEXT官方文档](https://theme-next.js.org/docs/getting-started/)
<h2 id="配置"><a name="t4"></a>配置</h2>

### 我目前更新使用 hexo 6.3.0版本，next 7.8.0版本

我更新了hexo，已经支持在根目录`source`文件夹下同步next配置，再也不用担心更改`themes/next/_config.yml`配置文件，换个电脑配置找不到了！
在根目录source文件夹下新建`_data`文件夹，并新建`next.yml`文件，然后把主题配置文件复制到这个文件中，修改这个文件中的配置项，主题配置就会生效，这个操作其实在`themes/next/_config.yml`文件中开头都有提到。

### 首先我们看看 `主题配置文件` 的配置 `\hexo\source\_data_\next.yml`

use_motion || motion: true  # 开启动画效果
use_motion: false # 关闭动画效果
1. 把页面中的动画效果取消，把enable改为对应的false改为true，然后hexo d -g，再进主页，问题就解决了，下面的length参数对应的是文章预览的文本长度，可以自己设置。
```yml
auto_excerpt:
  enable: false
  length: 150
```
> 这个配置在next新版本配置中已经被取消，需要自己安装一个插件
> ```
> npm install hexo-excerpt --save
> ```
> 在站点配置文件(不是主题的配置文件)_config.yml添加
> ```
> excerpt:			# 一定要顶格写，注意格式
   depth: 5			# 他的大小就是全文阅读预览长度设置
   excerpt_excludes: []
   more_excludes: []
   hideWholePostExcerpts: true  #是否隐藏整个帖子摘录
> ```
> 在主题配置文件中_config.yml里 `excerpt_description` 改为 `true`

2. 设置菜单项的显示文本和图标
NexT 使用的是 [Font Awesome](http://fontawesome.dashgame.com/) 提供的图标， Font Awesome 提供了 600+ 的图标，可以满足绝大的多数的场景，同时无须担心在 Retina 屏幕下图标模糊的问题。

3. 浏览页面的时候显示当前浏览进度
搜索关键字 scrollpercent ,把 false 改为 true。
```yml
# Scroll percent label in b2t button
scrollpercent: true
```

4. 添加顶部加载条
打开 `\source\_data_\next.yml` ，搜索关键字 `pace` ,设置为 `true` ,可以更换加载样式：

5. 修改文章底部的那个带#号的标签
```
修改模板`/themes/hexo-theme-next/layout/_macro/post.swig`，
搜索 `rel="tag">#`，将 `#` 换成`<i class="fa fa-tag"></i>`
```

6. 增加本地搜索功能
1> 在你站点的根目录下
```node
$ npm install hexo-generator-searchdb --save
```
2> 主题配置文件 `next.yml` 添加 `search` 选项，修改 `local_search` 选项：
```yml
search:  #不添加也可以，功能照用
  path: search.xml
  field: post
  format: html
  limit: 10000
local_search:
  enable: true
  trigger: auto #如果是auto，通过改变输入触发搜索。如果是手动，按回车键或搜索按钮触发搜索
  top_n_per_article: 1 #显示每篇文章的前n个结果，设置为-1显示所有结果
  unescape: false #将html字符串转义为可读的。
  preload: false #在页面加载时预加载搜索数据。
```

7. 自动摘录，主题配置文件：
```yml
excerpt_description: true ##自动摘录主页中的描述作为序言文本
read_more_btn: true #阅读更多按钮显示
```

8. 文章阴影设置
```scss
// 文章阴影
.post-block{
    padding: 25px;
    border-radius:5px;
    -webkit-box-shadow: 0 0 5px rgba(202, 203, 203, .5);
    -moz-box-shadow: 0 0 5px rgba(202, 203, 204, .5);
}
```

9. 文章顶部显示文章字数统计,阅读时长,总字数
安装 hexo-symbols-count-time 插件：
```npm
npm install hexo-symbols-count-time --save
```
编辑站点配置文件 hexo/_config.yml，添加如下内容：
```yml
symbols_count_time:
  symbols: true # 文章字数统计
  time: true # 文章阅读时间统计
  total_symbols: true # 站点总字数统计
  total_time: true  # 站点总阅读时间统计
```
修改主题配置文件 next.yml 中 symbols_count_time 选项：
```yml
symbols_count_time:
  separated_meta: true # 是否另起一行（true的话不和发表时间等同一行）
  item_text_post: true # 首页文章统计数量前是否显示文字描述（本文字数、阅读时长）
  item_text_total: true # 页面底部统计数量前是否显示文字描述（站点总字数、站点阅读时长）
  awl: 2 # 平均字长
  wpm: 275 # 每分钟阅读字数
```

10. 新建404公益界面，在`source`文件夹下新建`404/index.html`文件：
```html
<!DOCTYPE HTML>
<html>
<head>
  <meta http-equiv="content-type" content="text/html;charset=utf-8;"/>
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
  <meta name="robots" content="all" />
  <meta name="robots" content="index,follow"/>
  <link rel="stylesheet" type="text/css" href="https://qzone.qq.com/gy/404/style/404style.css">
</head>
<body>
  <script type="text/plain" src="https://www.qq.com/404/search_children.js"
          charset="utf-8" homePageUrl="/"
          homePageName="回到我的主页">
  </script>
  <script src="https://qzone.qq.com/gy/404/data.js" charset="utf-8"></script>
  <script src="https://qzone.qq.com/gy/404/page.js" charset="utf-8"></script>
</body>
</html>
```

11.我们再看看 `站点配置文件` 的配置 `F:\hexo\_config.yml`

```yml
# Hexo Configuration
## Docs: https://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/

# Site
title: blog station  #网站标题
subtitle: 天下事有难易乎？为之，则难者亦易矣；不为，则易者亦难矣。	#网站副标题
description: 郭雪辉博客  #网站描述
keywords: 郭雪辉博客 #网站的关键词。支持多个关键词。
author: 郭雪辉 #您的名字
language: zh-CN  #网站使用的语言。对于简体中文用户来说，使用不同的主题可能需要设置成不同的值，请参考你的主题的文档自行设置，常见的有 zh-Hans和 zh-CN。
timezone: Asia/Shanghai #网站时区。Hexo 默认使用您电脑的时区。请参考 时区列表 进行设置，如 America/New_York, Japan, 和 UTC 。一般的，对于中国大陆地区可以使用 Asia/Shanghai。
#其中，description主要用于SEO，告诉搜索引擎一个关于您站点的简单描述，通常建议在其中包含您网站的关键词。author参数用于主题显示文章的作者。

# URL
## Set your site url here. For example, if you use GitHub Page, set url as 'https://username.github.io/project'
url: http://gxhpersonal.github.io/blog #网址, 必须以 http:// 或 https:// 开头
root: /blog/ #网站根目录
permalink: :year/:month/:day/:title/  #文章的 永久链接 格式
permalink_defaults:  #永久链接中各部分的默认值
pretty_urls:  #改写 permalink 的值来美化 URL
  trailing_index: true # 是否在永久链接中保留尾部的 index.html，设置为 false 时去除
  trailing_html: true # 是否在永久链接中保留尾部的 .html, 设置为 false 时去除 (对尾部的 index.html无效)

# Directory
source_dir: source  #资源文件夹，这个文件夹用来存放内容。
public_dir: public #公共文件夹，这个文件夹用于存放生成的站点文件。
tag_dir: tags #标签文件夹
archive_dir: archives #归档文件夹
category_dir: categories #分类文件夹
code_dir: downloads/code #Include code 文件夹，source_dir 下的子目录
i18n_dir: :lang #国际化（i18n）文件夹
skip_render: #跳过指定文件的渲染。匹配到的文件将会被不做改动地复制到 public 目录中。您可使用 glob 表达式来匹配路径。

# Writing
new_post_name: :title.md #新文章的文件名称
default_layout: post #预设布局
auto_spacing: false #在中文和英文之间加入空格
titlecase: false #把标题转换为 title case
external_link: #在新标签中打开链接
  enable: true # Open external links in new tab
  field: site #对整个网站（site）生效或仅对文章（post）生效
  exclude: '' #需要排除的域名。主域名和子域名如 www 需分别配置
filename_case: 0 #把文件名称转换为 (1) 小写或 (2) 大写
render_drafts: false #显示草稿
post_asset_folder: false #启动 Asset 文件夹
relative_link: false #把链接改为与根目录的相对位址
future: true #显示未来的文章
highlight: #代码块的设置, 请参考 Highlight.js 进行设置
  enable: true
  line_number: true
  auto_detect: false
  tab_replace: ''
  wrap: true
  hljs: false
prismjs: #代码块的设置, 请参考 PrismJS 进行设置
  enable: false
  preprocess: true
  line_number: true
  tab_replace: ''

excerpt:
  depth: 4
  excerpt_excludes: []
  more_excludes: []
  hideWholePostExcerpts: true
  
# Home page setting
# path: Root path for your blogs index page. (default = '')
# per_page: Posts displayed per page. (0 = disable pagination)
# order_by: Posts order. (Order by date descending by default)
index_generator:
  path: ''
  per_page: 10
  order_by: -date

# Category & Tag
default_category: uncategorized #默认分类
category_map: #分类别名
tag_map: #标签别名

# Metadata elements
## https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta
meta_generator: true

# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
## Hexo 使用 Moment.js 来解析和显示时间
date_format: YYYY-MM-DD #日期格式
time_format: HH:mm:ss #时间格式
## updated_option supports 'mtime', 'date', 'empty'
updated_option: 'mtime' #当 Front Matter 中没有指定 updated 时 updated 的取值
 
# Pagination
## Set per_page to 0 to disable pagination
per_page: 10 #每页显示的文章量 (0 = 关闭分页功能)
pagination_dir: blog #分页目录

# Include / Exclude file(s)
## include:/exclude: options only apply to the 'source/' folder
include:
exclude:
ignore:

# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next
avatar: http://www.guoxh.com/blog/img/avatar.png

# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: git
  repo: https://github.com/gxhpersonal/blog.git
  branch: gh-pages

# 百度分享服务
baidushare: true

# 多说热评文章 true 或者 false
# duoshuo_hotartical: true

# search:
#   path: search.xml
#   field: post
#   format: html
#   limit: 10000

live2d:
  enable: true
  scriptFrom: local
  pluginRootPath: live2dw/
  pluginJsPath: lib/
  pluginModelPath: assets/
  tagMode: false
  debug: false
  model:
    use: live2d-widget-model-wanko
  display:
    position: left
    width: 100
    height: 150
  mobile:
    show: true

```

12.如果遇到本地生成样式没问题，上线样式错乱，先执行：`hexo clean`，再执行`hexo d -g`

13.文章指定摘录段落&&修改阅读全文样式

next 在需要显示摘要的地方加上 `<!--more-->` ，就不会显示全文，在`\hexo\source\_data_\style\styles.styl`中写入下面内容，修改默认的 Read More 按钮样式：
```scss
// [Read More]按钮样式
.post-button .btn {
    color: #555 !important;
    background-color: rgb(255, 255, 255);
    border-radius: 3px;
    font-size: 15px;
    box-shadow: inset 0px 0px 10px 0px rgba(0, 0, 0, 0.35);
    border: none !important;
    transition-property: unset;
    padding: 0px 15px;
}
.post-button .btn:hover {
    color: rgb(255, 255, 255) !important;
    border-radius: 3px;
    font-size: 15px;
    box-shadow: none;
    background-image: linear-gradient(90deg, #ff5200 0%, #ffd600 50%, #ff5200 100%);
}
```

* 上面修改样式的配置都可以自己自定义，只需要在F12中找到对应类名，在`\hexo\source\_data_\style\styles.styl`文件中添加对应类名想要的样式就好了