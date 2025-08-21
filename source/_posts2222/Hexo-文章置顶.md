---
title: Hexo文章置顶
index_img: /img_index/index/20190118-001.jpg
desc: Hexo博客站内文章添加置顶功能，top置顶
categories:
  - [Hexo]
tags: [Hexo, Hexo插件]
keywords: hexo, next, Hexo, 美化, 插件, 博客, blog, hexo-generator-index, 置顶, top
abbrlink: 18391
date: 2019-01-18 14:49:25
updated: 2019-01-18 14:49:25
---

<img src="/images/article_zhiding.png" style="border:1.5px solid blue"/>

#### 背景
{% note success %}
博文添加文章置顶功能
{% endnote %}

写了很多博客，总有几篇是自己想要指定的文章，上线找了很多发现都是老方法（安装***hexo-generator-index***），但是发现不管用，一般都是版本太老的原因。于是开始搜索新的插件，找到了***hexo-generator-index-pin-top***这个插件。

<!--more-->
<hr />

#### 正文
只要3步就可以让博文置顶，不多说，直接搞：

> 第一步：安装插件

在blog的根目录，执行如下命令，如果安装***hexo-generator-index***，请先进行卸载。
```
npm uninstall hexo-generator-index --save
npm install hexo-generator-index-pin-top --save
```

> 第二步：博文top置顶

找到需要置顶的文章，在文件顶部加上top: true即可，来个栗子：
```
---
title: Hexo文章置顶
comments: false
date: 2019-04-13 14:49:25
updated: 2019-04-13 14:49:25
desc: Hexo文章添加置顶功能
categories:
  [hexo]
tags: [hexo]
top: true
---
```

> 第三步：图标

实现1、2步之后，重新启动server之后，发现文章已经置顶了，但是没有图标是不是很尴尬。别怕，找到下面的文件，搜索***<div class="post-meta">***标签，并把代码加进去。
文件：/blog/themes/next/layout/_macro/post.swig
```
{% if post.top %}
  <i class="fa fa-thumbs-up"></i>
  <font color="red">置顶</font>
  <span class="post-meta-divider">|</span>
{% endif %}
```
图标不好看的也可以自己进行更改，替换***fa-thumb-tack***即可，还可以把图标的颜色进行更改。***hexo s***重新启动server查看效果吧。
图标选取地址：[自选图标](https://fontawesome.com/cheatsheet?from=io)

#### 结束

到这里就完成博文置顶的效果，尝试之后，是不是***so easy***。
