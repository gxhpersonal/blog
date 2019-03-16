---
title: hexo集成live2D动画
date: 2019-03-15 10:12:47
tags: blog
categories: blog
---

### 1.hexo博客文件目录下安装live2D插件，执行下面命令：
```git bash
npm install --save hexo-helper-live2d
```

### 2.下载live2D动画模型，下面展示了动画列表
>   live2d-widget-model-chitose
    live2d-widget-model-z16
    live2d-widget-model-epsilon2_1
    live2d-widget-model-gf
    live2d-widget-model-haru/01
    live2d-widget-model-haru/02
    live2d-widget-model-haruto
    live2d-widget-model-hibiki
    live2d-widget-model-hijiki
    live2d-widget-model-izumi
    live2d-widget-model-koharu
    live2d-widget-model-miku
    live2d-widget-model-ni-j
    live2d-widget-model-nico
    live2d-widget-model-nietzsche
    live2d-widget-model-nipsilon
    live2d-widget-model-nito
    live2d-widget-model-shizuku
    live2d-widget-model-tororo
    live2d-widget-model-tsumiki
    live2d-widget-model-unitychan
    live2d-widget-model-wanko

执行下面命令，安装具体的动画模型包：
```
npm install [live2d-widget-model-wanko]
```
[具体动画模型展示](https://huaji8.top/post/live2d-plugin-2.0/)

### 3.最后，修改站点配置文件`_config.yml`,添加下面配置：
```
# Live2D
## https://github.com/EYHN/hexo-helper-live2d
live2d:
  enable: true #隐藏/显示
  pluginRootPath: live2dw/    #插件在站点上的根路径(相对)
  pluginJsPath: lib/          #与插件根目录相关的JavaScript路径(相对)
  pluginModelPath: assets/    #与插件根(相对)相关的模型路径
  scriptFrom: local # Default
  # scriptFrom: jsdelivr # jsdelivr CDN
  # scriptFrom: unpkg # unpkg CDN
  # scriptFrom: https://cdn.jsdelivr.net/npm/live2d-widget@3.x/lib/L2Dwidget.min.js # Your custom url
  tagMode: false  #是否只替换live2d标签而不是注入到所有页面
  log: false #是否在控制台显示日志
  model:
    use: live2d-widget-model-wanko #安装的模型包名称
```
#### hexo d -g执行即可
[插件GitHub地址](https://github.com/EYHN/hexo-helper-live2d)