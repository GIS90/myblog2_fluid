---
title: Hexo+Next博客添加喵星人
index_img: /img_index/index/20181223-001.gif
desc: 给博客注入一只博客添加喵星人（live2d）【Hexo+Next】
categories:
  - [Hexo]
tags: [Hexo, Hexo美化, Hexo插件]
keywords: hexo, next, Hexo, 美化, 插件, 博客, blog, 分类页, live2d, 喵星人
abbrlink: 4445
date: 2018-12-23 11:09:37
updated: 2018-12-23 11:09:37
---



{% note success %}
如图，四步让给你博客入住喵星人。
{% endnote %}

<img src="article_live2d.png" style="border:1.5px solid red;height: 200px;"/>

<!--more-->
<hr />

### 1、安装hexo-helper-live2d

```
npm install hexo-helper-live2d --save
```

### 2、下载model模型

- 在博客根目录创建一文件夹，命名live2d_models。
- 打开[Live2D模型](https://github.com/xiazeyu/live2d-widget-models)，把项目下载到本地。
- 把packages下的所有文件都copy到新建目录live2d_models下，每一个目录都对应一个模型。

### 3、配置博客config

打开博客的_config.yml文件，不是Next主题的配置文件，看好了。。。之后，把下面代码追加到配置文件中。
```
# 动漫live2d
live2d:
  enable: true
  scriptFrom: local
  pluginRootPath: live2dw/
  pluginJsPath: lib/
  pluginModelPath: assets/
  tagMode: false
  debug: false
  model:
    use: ./live2d_models/live2d-widget-model-z16
  display:
    position: right
    width: 450
    height: 750
    hOffset: 10
    vOffset: -20
  mobile:
    show: true
  react:
    opacity: 0.8
```
配置文件中live2d->model->use：./live2d_models/live2d-widget-model-z16，其中***live2d-widget-model-z16***是模型名称，只要更换新建目录live2d_models下的模型名称就可以改变。

### 4、重启server
```
hexo g -d
hexo s
```
重启服务&&刷新页面，会发现一只喵星人出现在你的博客上。

### 5、配置文件详解

#### 5.1、插件配置
```
enable          是否启用插件
scriptFrom      未知
pluginRootPath	插件在站点上根目录的相对路径
pluginJsPath	脚本文件相对与插件根目录路径
pluginModelPath	模型文件相对与插件根目录路径
tagMode	Boolean	标签模式, 是否仅替换***live2d tag***标签而非插入到所有页面中
debug	        调试模式, 控制是否在控制台输出日志
```
说明：***hexo g***之后，会在blog/public目录下自动生成一个新文件夹***live2dw***，里面存放发布后站点模型。

#### 5.2、模型文件存储目录
```
model:
  use: ./live2d_models/live2d-widget-model-z16
```

#### 5.3、启用

启用显示的模型名称，***live2d_models***目录下每一个目录都是一个模型。
```
display:
  position: right
  width: 450
  height: 750
  hOffset: 10
  vOffset: -20
```

#### 5.4、模型位置
```
position  模型的位置，有left&&right
width     模型的宽度
height    模型的高度
hOffset   模型的水平偏移
vOffset   模型的垂直偏移
```

#### 5.5、移动端是否进行显示

```
mobile:
  show: true
```

#### 5.6、透明度
透明度设置：0～1 透明～不透明
```
react:
  opacity: 0.8
```


### 6、学习参考

- live2d官网：https://github.com/EYHN/hexo-helper-live2d
- live2d模型：https://github.com/xiazeyu/live2d-widget-models/tree/master/packages
