---
title: hexo强力搭建github博客
date: 2016-09-14 16:28:35
tags:
---

# 一、 前言
不知道亚林兄借鉴谁的，但是我是借鉴亚林兄的，版权所有，归他

<!-- more -->

如果是小小白，可以先花时间去了解下：
* [Git](http://git-scm.com/book/zh/v2)
* [GitHub](https://github.com/)
* [GitHub Pages](https://pages.github.com/)
* [Hexo](https://hexo.io/zh-cn/)
* [Markdown](http://www.appinn.com/markdown/#autoescape)

# 二、 必要配置
## 2.1 GitHub Pages 仓库
### 2.1.1 创建对应仓库
在自己的GitHub账号下创建一个新的仓库，命名为blog。

## 2.2 Git
### 2.2.1 安装 Git
在windows下安装git比较常用的有两种方式：
1. [Git 官方版本的安装](http://git-scm.com/download/win)
2. [GitHub for Windows](https://desktop.github.com/)

### 2.2.2 配置 Git
当安装完Git应该做的第一件事情就是设置用户名称和邮件地址。这样做很重要，因为每一个Git的提交都会使用这些信息，并且它会写入你的每一次提交中，不可更改：

```bash
$ git config --global user.name "username"
$ git config --global user.email "username@example.com"
```

### 2.2.3 相关资料
* [安装 Git](http://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%AE%89%E8%A3%85-Git)
* [配置 Git](http://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%88%9D%E6%AC%A1%E8%BF%90%E8%A1%8C-Git-%E5%89%8D%E7%9A%84%E9%85%8D%E7%BD%AE)

## 2.3 Git 与 GitHub
### 2.3.1 git与github的区别
这里，我们要区分清楚git与github。

git是一个版本控制的工具，而github有点类似于远程仓库，用于存放用git管理的各种项目。

### 2.3.2 与github建立联系
为了能够在本地使用git管理github上的项目，需要进行一些配置，这里介绍SSH的方法。

#### 2.3.2.1 检查电脑是否已经有SSH keys。
``` bash
$ ls -al ~/.ssh
# Lists the files in your .ssh directory, if they exist
```

默认情况下，public keys的文件名是以下的格式之一：id_dsa.pub、id_ecdsa.pub、id_ed25519.pub、id_rsa.pub。因此，如果列出的文件有public和private钥匙对（例如id_ras.pub和id_rsa），证明已存在SSH keys。

#### 2.3.2.2 如果没有SSH key，则生成新的SSH key。
```bash
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
# Creates a new ssh key, using the provided email as a label
```
之后一路回车即可。

#### 2.3.2.3 向ssh-agent添加key。
首先确保ssh-agent可运行：
```bash
# start the ssh-agent in the background
$ ssh-agent -s
```
然后添加SSH key：
```bash
$ ssh-add ~/.ssh/id_rsa
```

#### 2.3.2.4 在GitHub添加SSH key。
首先，拷贝key：
```bash
clip < ~/.ssh/id_rsa.pub
# Copies the contents of the id_rsa.pub file to your cllipboard
```
然后，在GitHub右上方点击头像，选择"Settings"，在左边的"SSH and GPG keys"侧边栏选择"New SSH key"。接着粘贴key，点击"Add key"按钮。最后，测试链接：
```bash
$ ssh -T git@github.com
# Attempts to ssh to GitHub
```
如果你看到：
```bash
The authenticity of host 'github.com (207.97.227.239)' can't be established.
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)?
```
就键入：yes。之后将会看到如下信息：
```bash
Hi username! You've successfully authenticated, but GitHub does not
provide shell access.
```

### 2.3.3 相关资料
* [Generating SSH keys](https://help.github.com/articles/generating-ssh-keys/)

## 2.4 Hexo
### 2.4.1 安装Hexo
安装Hexo相当简单。在安装之前，必须检查电脑中是否已经安装下列应用程序：
* [Node.js](http://nodejs.org/)
* [Git](http://git-scm.com/)
如果您的电脑中已经安装上述必备程序，那么恭喜您！接下来只需要使用 npm 即可完成 Hexo 的安装。
```bash
$ npm install -g hexo-cli
```

### 2.4.2 使用Hexo建站
安装完后，在你喜欢的文件夹内（例如f：\Blog），点击鼠标右键选择Git bash，输入以下指令：

```bash
$ hexo init
```
该命令会在目标文件夹内建立网站所需要的所有文件。接下来是安装依赖包：

```bash
$ npm install
```

这样，我们就已经搭建起本地的Hexo博客了。可以先执行以下命令（在对应文件夹下），然后再浏览器输入localhost:4000查看。

```bash
$ hexo generate
$ hexo server
```

这个博客只是本地的，别人是浏览不了的，之后需要部署到GitHub上。

### 2.4.3 相关资料
* [Hexo 官方文档](https://hexo.io/zh-cn/docs/)

## 三、一般的搭建方法
在上面，我们已经配置好了所需的所有东西，也成功地搭建了一个本地Hexo博客。现在，需要使用GitHub Pages搭建一个别人能够访问的Hexo博客了。

### 3.1 使用默认theme
我们继续使用上面的文件夹f:\Blog（也可以新建一个文件夹重新生成），然后编辑该文件夹下的_config.yml。

默认生成的_config.yml：
```bash
# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type:
```

修改后的_config.yml：
```bash
deploy:
  type: git
  repo: 对应仓库的https地址（可以在GitHub对应的仓库中复制）
  branch: 分支（User Pages为master，Project Pages为gh-pages）
```
* 注意：冒号后面要加上一个空格，否则会报错。

如果你的仓库不是使用的github的主仓库，则需要改_config.yml的root：

```bash
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://yoursite.com
root: /blog/
permalink: :year/:month/:day/:title/
permalink_defaults:
```

为了能够使Hexo部署到GitHub上，需要安装一个插件：

```bash
$ npm install hexo-deployer-git --save
```

然后，执行下列指令即可完成部署：

```bash
$ hexo generate
$ hexo deploy
```
如果 hexo deploy 报错，请在Git Bash里边打开重新走上面两步

之后，可以通过在浏览器键入：username.github.io/blog进行浏览，开心吧~

### 3.2 其他theme
如果想要使用其他主题，可以使用git clone将别人的主题拷贝到D:\Hexo\themes下，然后将_config.yml中的theme: landscape改为对应的主题名字。

详细步骤可以参考网上的指南。

提示：上述搭建步骤大量参考了yangruihan的博客，如果你对搭建过程还有疑问，可以查看他的博文[如何利用GitHub Pages和Hexo快速搭建个人博客](http://sunwhut.com/2015/10/30/buildBlog/)

### 3.3 发表博文

辛苦了这么久，终于回到我们搭建博客最初的目标–写作，现在来看看怎么写博文并发表吧。

#### 3.3.1 新建博文

我们可以使用命令新建一篇博文,使用 Git Shell 进入 Hexo 文件夹，输入以下命令：

```bash
hexo new "文章题目"
```

* 命令执行完后，就会发现在 Hexo\source_posts 目录中多了一个文件博文名.md，这就是我们刚才新建的博文。

* 此外，我们也可以直接进入 Hexo\source_posts 目录中，右键新建一个文本文档，将名字改为博文名.md,这样也新建了一篇博文。

#### 3.3.2 新建页面

```bash
hexo new page "页面名称"

```
* 命令执行完后，就会发现在在 Hexo\source 目录中多了一个文件夹，里面还有一个index.md,这就代表我们新建了一个页面。

### 3.4 写博文

用文本编辑器打开上面新建的博文，如下所示：
```bash
---
title: GitHub Pages + Hexo搭建博客
date: 2016-08-07 17:04:35
tags:
categories: Hexo
---
```
* 新建的页面略有不同，没有tags和categories标签。

* 三个”-“后面就是博文的正文内容，接下来就是正儿八经地撰写博文了。
* 因为我们的博文都是用Markdown语言写的，所以首先，你需要一个好用的Markdown编辑器。其实好用的Markdown编辑器一大堆，这里就给大家推荐两个，如果你用的不习惯也可以换其它的。
* 本地编辑器：[Haroopad](http://sunwhut.com/2015/10/30/buildBlog/),非常小众的一款Markdown编辑器，左边编辑右边实时预览效果，非常轻便；
* 在线编辑器：[MaHua](http://mahua.jser.me/),也是比较小众的一款Markdown编辑器，但效果确实很棒。
* 现在你可以打开新建的博文了，然而还不造怎么下手对吧。其实很简单，除了特殊格式，其它的你就当做在word里面写文章就行了，具体请看这里的Markdown教程：[Markdown——入门指南](http://www.jianshu.com/p/1e402922ee32/#).
### 3.5 发博文

博文写好了，依然在 Git Shell 中进入 Hexo 文件夹，执行下面几条命令，将博客部署到 GitHub 上：

```bash
hexo clean
hexo generate (若要本地预览就先执行 hexo server)
hexo deploy
```

```bash
快捷命令：
hexo g == hexo generate
hexo d == hexo deploy
hexo s == hexo server
hexo n == hexo new
```
还能组合使用，如：

```bash
hexo d -g
```
刷新你的个人博客，就可以看到新鲜出炉的博文了。

## 四、安装主题

### 4.1 选择主题

我们刚才使用Hexo生成的博客使用的是Hexo的默认主题：Landscape。

不过hexo 给我们提供了大量的主题，[Themes·Hexo](https://github.com/hexojs/hexo/wiki/Themes) 我选择的是这个主题：NexT。

### 4.2 安装NexT主题

Hexo 有两份主要的配置文件`_config.yml`，一份位于站点根目录下，另一份位于主题目录下。为了描述方便，在以下说明中，将前者称为 `站点配置文件`，后者称为 `主题配置文件`。

#### 4.2.1 下载 NexT 主题

使用 Git Shell 进入 Hexo 文件夹，输入以下两条命令：
```bash
cd Hexo
git clone https://github.com/iissnan/hexo-theme-next themes/next
```
#### 4.2.2 启用NexT主题

下载完成后，打开 `站点配置文件`，找到 theme 字段，并将其值更改为 next。

#### 4.2.3 验证主题是否启用

执行上面发博文的命令($ hexo d -g)，刷新你的个人博客，就能看到你设置的主题是否启用。

### 4.3 设置NexT主题和第三方服务
安装完NexT之后，还是发现不够漂亮对不对，所以下面我们来慢慢地润色你的个人博客。

#### 4.3.1 选择样式

NexT默认的样式其实也比较丑，幸好作者提供了一款十分漂亮的样式:Mist。启用 Mist 很简单，仅需在 `主题配置文件`中将 #scheme: Mist 前面的 # 注释去掉即可。

#### 4.3.2 菜单设置

菜单配置在 主题配置文件 的 menu，下面是菜单配置示例：

```bash
menu:
  #home: /
  archives: /archives
  about: /about
  categories: /categories
  tags: /tags
  #commonweal: /404.html
```
* 除了home和archives菜单主题自带，其他菜单需要自己建文件夹，非常简单的还是；
其它的很多在[NexT官方文档](http://theme-next.iissnan.com/)里面已经讲的很清楚了,可以自己去研究研究。
## 五、 优化部署与管理

### 5.1 概述
Hexo部署到GitHub上的文件，是.md（你的博文）转化之后的.html（静态网页）。因此，当你重装电脑或者想在不同电脑上修改博客时，就不可能了（除非你自己写html o(^▽^)o ）。

其实，Hexo生成的网站文件中有.gitignore文件，因此它的本意也是想我们将Hexo生成的网站文件存放到GitHub上进行管理的（而不是用U盘或者云备份啦。这样，不仅解决了上述的问题，还可以通过git的版本控制追踪你的博文的修改过程，是极赞的。

但是，如果每一个GitHub Pages都需要创建一个额外的仓库来存放Hexo网站文件，我感觉很麻烦（10个项目需要20个仓库(ˉ▽ˉ；)...）。

所以，我利用了分支！！！

简单地说，每个想建立GitHub Pages的仓库，起码有两个分支，一个用来存放Hexo网站的文件，一个用来发布网站。

下面以我的博客作为例子详细地讲述。


### 5.2 我的博客搭建流程

1. github创建仓库，blog
2. 在本地新建的/blog文件夹中运行Git Bash,输入git init(意思是在当前文件夹下创建git环境与github联系起来[个人观点，哈哈]);
3. 接着命令行输入git remote add origin https://github.com/用户名/仓库名.git
4. 依次执行git add .、git commit -m "..."、git push origin master提交网站相关的文件；
PS: 我是在gh-pages分支上改的，所以提交代码时会提示github上没有master分支，然后点击确定会帮你创建一个master分支，就可以提交成功了；

这样一来，在GitHub上的gxhpersonal.github.io仓库就有两个分支，一个master分支用来存放网站的原始文件，一个gh-pages分支用来存放生成的静态网页。完美( •̀ ω •́ )y！

### 5.3 我的博客管理流程
#### 5.3.1 日常修改
在本地对博客进行修改（添加新博文、修改样式等等）后，通过下面的流程进行管理：
> 个人习惯：
1. 先依次执行git add .、git commit -m "..."、git push origin master指令将改动推送到GitHub（此时当前分支应为master）；
2. 然后才执行hexo generate -d发布网站到gh-pages分支上。
3. PS:其实gh-pages分支不用管的，当你运行hexo d -g 的时候就会把生成的博客代码自动提交到gh-pages分支;

虽然两个过程顺序调转一般不会有问题，不过逻辑上这样的顺序是绝对没问题的（例如突然死机要重装了，悲催....的情况，调转顺序就有问题了）。

#### 5.3.2 本地资料丢失
当重装电脑之后，或者想在其他电脑上修改博客，可以使用下列步骤：

1. 使用git clone git@github.com:gxhpersonal/blog.git拷贝仓库（默认分支为master）；
2. 在本地新拷贝的blog文件夹下通过Git bash依次执行下列指令：npm install hexo、npm install、npm install hexo-deployer-git（记得，不需要hexo init这条指令）。