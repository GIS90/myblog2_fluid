---
title: Hexo博文展示并排等多样式图片
index_img: /img_index/index/20181212-001.png
desc: hexo博文展示并排等多样式图片，优化博客图片的存放方式，内置功能
categories:
  - [Hexo]
tags: [Hexo, Hexo美化]
keywords: hexo, next, Hexo, 美化, 插件, 博客, blog, gp, 多排图片
abbrlink: 8728
date: 2018-12-12 14:05:52
updated: 2018-12-12 14:05:52
---


{% note success %}
用markdown语法去写博文的时候，发现图片怎么调试怎么不能并排，博客采用Hexo+Next搭建，上网各种查资料，终于解决，在此记录上分享给大家。
{% endnote %}

<!--more-->

<hr />

使用***gp***标签引用要展示的图片地址，这个并列展示效果可以在，***theme/next/scripts/tags/group-pictures.js***文件中进行选取，话说多说，直接上代码：
```
{% gp 3-3 %}
<img src="t1.jpeg" width="450" alt="图片说明"/>
<img src="t2.jpeg" width="450" alt="图片说明"/>
<img src="t3.jpeg" width="800" alt="图片说明"/>
{% endgp %}
```
博客正文显示需要修改一下文件代码，***themes\next\source\css\_common\components\tags\group-pictures.styl***样式文件中更改代码如下：
```
.page-post-detail .post-body .group-picture-column {
  // float: none;
  margin-top: 10px;
  // width: auto !important;
  img { margin: 0 auto; }
}
```
设置好了之后，***hexo g***一下，刷新一下就会看到效果。
在***gp***标签可以使用***img***标签、***\!\[\]\(\)***也可以这样显示图片，使用***img***标签的好处就在于自定义样式。

问题解决原文：https://github.com/iissnan/hexo-theme-next/issues/395
