---
title: hexo next主题配置优化
date: 2016-11-22 13:44:16
tags:
categories: blog
---
### NEXT主题官方文档，操作扩展都非常详细，有兴趣可以看一看 
[NEXT主题配置](http://theme-next.iissnan.com/theme-settings.html)   
[NEXT官方文档](https://theme-next.js.org/docs/getting-started/)
<h2 id="配置"><a name="t4"></a>配置</h2>

### 首先我们看看 `主题配置文件` 的配置 `F:\hexo\themes\next\_config.yml`
```
use_motion || motion: true  # 开启动画效果
use_motion: false # 关闭动画效果
1. 把页面中的动画效果取消，把enable改为对应的false改为true，然后hexo d -g，再进主页，问题就解决了，下面的length参数对应的是文章预览的文本长度，可以自己设置。
auto_excerpt:
  enable: false
  length: 150

2. 设置菜单项的显示文本和图标
NexT 使用的是 [Font Awesome](http://fontawesome.dashgame.com/) 提供的图标， Font Awesome 提供了 600+ 的图标，可以满足绝大的多数的场景，同时无须担心在 Retina 屏幕下图标模糊的问题。

3. 浏览页面的时候显示当前浏览进度
搜索关键字 scrollpercent ,把 false 改为 true。
# Scroll percent label in b2t button
scrollpercent: true

4. 添加顶部加载条
打开 themes/next/_config.yml ，搜索关键字 pace ,设置为 true ,可以更换加载样式：

5. 修改文章底部的那个带#号的标签
修改模板/themes/hexo-theme-next/layout/_macro/post.swig，
搜索 rel="tag">#，将 # 换成<i class="fa fa-tag"></i>

6. 增加本地搜索功能
1> 在你站点的根目录下
$ npm install hexo-generator-searchdb --save
2> 打开 Hexo 站点配置文件 _config.yml,添加配置
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
3> 打开 themes/next/_config.yml ,搜索关键字 local_search ,enable 设置为 true：
local_search:
  enable: true
```

### 我们再看看 `站点配置文件` 的配置 `F:\hexo\_config.yml`
<pre class="prettyprint" name="code"><code class="hljs avrasm has-numbering"><span class="hljs-preprocessor"># Hexo Configuration</span>
<span class="hljs-preprocessor">## Docs: https://hexo.io/docs/configuration.html</span>
<span class="hljs-preprocessor">## Source: https://github.com/hexojs/hexo/</span>
<span class="hljs-preprocessor"># Site 网站</span>
<span class="hljs-label">title:</span> 为学   <span class="hljs-preprocessor">#网站标题</span>
<span class="hljs-label">subtitle:</span> 天下事有难易乎？为之，则难者亦易矣；不为，则易者亦难矣。   <span class="hljs-preprocessor">#网站副标题</span>
<span class="hljs-label">description:</span> 天下事有难易乎？为之，则难者亦易矣；不为，则易者亦难矣。   <span class="hljs-preprocessor">#网站描述</span>
<span class="hljs-label">author:</span> willxue   <span class="hljs-preprocessor">#您的名字</span>
<span class="hljs-label">language:</span> <span class="hljs-built_in">zh</span>-CN   <span class="hljs-preprocessor">#网站使用的语言</span>
<span class="hljs-label">timezone:</span>           <span class="hljs-preprocessor">#网站时区。Hexo 默认使用您电脑的时区</span>

<span class="hljs-preprocessor"># URL 网址</span>
<span class="hljs-preprocessor">## 如果您的网站存放在子目录中，例如 http://yoursite.com/blog，则请将您的 url 设为 http://yoursite.com/blog 并把 root 设为 /blog/。</span>
<span class="hljs-label">url:</span> http://willxue<span class="hljs-preprocessor">.top</span>
<span class="hljs-label">permalink:</span> :year/:month/:day/:title/    <span class="hljs-preprocessor">#生成文件名字的格式我改成blog/:title:year:month:day/</span>
<span class="hljs-label">permalink_defaults:</span>

<span class="hljs-preprocessor"># Directory 目录配置</span>
<span class="hljs-label">source_dir:</span> source   <span class="hljs-preprocessor">#源文件夹，这个文件夹用来存放内容。</span>
<span class="hljs-label">public_dir:</span> public   <span class="hljs-preprocessor">#公共文件夹，这个文件夹用于存放生成的站点文件。</span>
<span class="hljs-label">tag_dir:</span> tags   <span class="hljs-preprocessor">#标签文件夹</span>
<span class="hljs-label">archive_dir:</span> archives   <span class="hljs-preprocessor">#归档文件夹</span>
<span class="hljs-label">category_dir:</span> categories   <span class="hljs-preprocessor">#分类文件夹</span>
<span class="hljs-label">code_dir:</span> downloads/code    <span class="hljs-preprocessor">#nclude code 文件夹</span>
<span class="hljs-label">i18n_dir:</span> :lang   <span class="hljs-preprocessor">#国际化（i18n）文件夹</span>
<span class="hljs-label">skip_render:</span>   <span class="hljs-preprocessor">#跳过指定文件的渲染，您可使用 glob 表达式来匹配路径。</span>

<span class="hljs-preprocessor"># Writing 文章</span>
<span class="hljs-label">new_post_name:</span> :title<span class="hljs-preprocessor">.md</span>   <span class="hljs-preprocessor"># 新建文章默认文件名</span>
<span class="hljs-label">default_layout:</span> post   <span class="hljs-preprocessor"># 默认布局</span>
<span class="hljs-label">titlecase:</span> false   <span class="hljs-preprocessor"># Transform title into titlecase</span>
<span class="hljs-label">external_link:</span> true   <span class="hljs-preprocessor"># 在新标签中打开一个外部链接，默认为true</span>
<span class="hljs-label">filename_case:</span> <span class="hljs-number">0</span>   <span class="hljs-preprocessor">#转换文件名，1代表小写；2代表大写；默认为0，意思就是创建文章的时候，是否自动帮你转换文件名，默认就行，意义不大。</span>
<span class="hljs-label">render_drafts:</span> false   <span class="hljs-preprocessor">#是否渲染_drafts目录下的文章，默认为false</span>
<span class="hljs-label">post_asset_folder:</span> false   <span class="hljs-preprocessor">#启动 Asset 文件夹</span>
<span class="hljs-label">relative_link:</span> false   <span class="hljs-preprocessor">#把链接改为与根目录的相对位址，默认false</span>
<span class="hljs-label">future:</span> true   <span class="hljs-preprocessor">#显示未来的文章，默认false</span>
<span class="hljs-label">highlight:</span>   <span class="hljs-preprocessor">#代码块的设置 </span>
  enable: true
  line_number: true
  auto_detect: false
  tab_replace:

<span class="hljs-preprocessor"># Category &amp; Tag   分类和标签的设置</span>
<span class="hljs-label">default_category:</span> uncategorized   <span class="hljs-preprocessor">#默认分类</span>
<span class="hljs-label">category_map:</span>   <span class="hljs-preprocessor">#分类别名</span>
<span class="hljs-label">tag_map:</span>   <span class="hljs-preprocessor">#标签别名</span>

<span class="hljs-preprocessor"># Date / Time format</span>
<span class="hljs-preprocessor">## Hexo uses Moment.js to parse and display date</span>
<span class="hljs-preprocessor">## You can customize the date format as defined in</span>
<span class="hljs-preprocessor">## http://momentjs.com/docs/#/displaying/format/</span>
<span class="hljs-label">date_format:</span> YYYY-MM-DD
<span class="hljs-label">time_format:</span> HH:mm:ss

<span class="hljs-preprocessor"># Pagination 分页</span>
<span class="hljs-preprocessor">## Set per_page to 0 to disable pagination</span>
<span class="hljs-label">per_page:</span> <span class="hljs-number">10</span>   <span class="hljs-preprocessor">#每页显示的文章量 (0 = 关闭分页功能)</span>
<span class="hljs-label">pagination_dir:</span> page   <span class="hljs-preprocessor">#分页目录</span>

<span class="hljs-preprocessor"># Extensions</span>
<span class="hljs-preprocessor">## Plugins: https://hexo.io/plugins/</span>
<span class="hljs-preprocessor">## Themes: https://hexo.io/themes/</span>
<span class="hljs-label">theme:</span> next

<span class="hljs-label">feed:</span>
  type: atom       <span class="hljs-preprocessor">#feed 类型 (atom/rss2)</span>
  path: atom<span class="hljs-preprocessor">.xml</span>   <span class="hljs-preprocessor">#rss 路径</span>
  limit: <span class="hljs-number">20</span>        <span class="hljs-preprocessor">#在 rss 中最多生成的文章数(0显示所有)</span>

<span class="hljs-preprocessor"># Deployment</span>
<span class="hljs-preprocessor">## Docs: https://hexo.io/docs/deployment.html</span>
<span class="hljs-label">deploy:</span> 
<span class="hljs-label">type:</span> git 
  repository: https://github<span class="hljs-preprocessor">.com</span>/imwillxue/imwillxue<span class="hljs-preprocessor">.github</span><span class="hljs-preprocessor">.com</span><span class="hljs-preprocessor">.git</span> 
  branch: master</code></pre>