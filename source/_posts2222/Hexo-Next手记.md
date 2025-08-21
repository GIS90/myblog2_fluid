---
title: Hexo+Next博客写作手札
index_img: /img_index/index/20190118-001.png
categories:
  - [Hexo]
tags: [Hexo]
top: false
abbrlink: 28748
date: 2019-01-18 10:48:57
updated: 2019-01-18 10:48:57
desc: 关于blog的博文写作，记录一些常用的模板
keywords: hexo, next, Hexo, 美化, 插件, 博客, blog, 手记, 自定义, 样式, js代码
---

{% note success %}
<font color="red" size="4">**简介**</font>
记录一些**Hexo、Next**常用文件的一些信息。
{% endnote %}

<!--more-->
<hr />

{% note info %}
### <font color="red">自定义样式文件</font>

作用：
**记录博客自定义的样式，以class方式进行css样式编写。**

位置：
**blog/themes/next/source/css/_custom/custom.styl**

{% endnote%}

{% note info %}
### <font color="red">自定义html代码</font>

作用：
**引入自动html的相关代码，标签、css、script都可以在这里编写。**

位置：
**blog/themes/next/layout/_custom/custom.swig**

建议：
**虽然这个文件可以引入自定义的html代码，但是建议把标签、css、js代码分开来写。**
**html**：写在对应的模块swig文件。
**css**：写在上面的自定义样式文件。
**js**：blog/themes/next/source/js/src目录下创建新js文件，在blog/themes/next/layout/_layout.swig文件中进行引用。

{% endnote%}
