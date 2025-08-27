---
title: 鼠标自定义样式
index_img: /img_index/index/20190610-001.jpg
categories:
  - [前端开发]
tags: [Hexo, Hexo美化, 前端其他分类]
abbrlink: 3035
date: 2019-06-10 19:03:44
updated: 2019-06-10 19:03:44
desc: 关于修改博客鼠标自定义样式的教程
keywords: Hexo, 美化, 前端, 鼠标, 自定义, 样式, 美化, blog
---

### 背景
{% note success %}
**自定义鼠标样式**，将blog的优化进行到底，让每天更加美化。
{% endnote %}

<!--more-->

<hr />

### 正文

- {% btn /images/custome_style.7z, 压缩包, download fa-lg fa-fw, 压缩包 %}点击下载样式文件，把**.cur**样式文件copy到**blog/public**目录下。

- 打开自定义样式文件：**blog/themes/next/source/css/_custom/custom.styl**，加入以下代码：

{% code %}
body {
    background:url(/images/background.jpg);
    cursor: url('/images/shubiao_guangmingxi.cur'), auto !important;
}
{% endcode %}

我只定义了正常状态的鼠标样式，其他状态的样式根据自己的喜欢定义即可。

### 学习

详细教程：https://juejin.im/post/5b4876b66fb9a04f9e230066

{% note info %}
### 说明
鼠标样式文件我是从网上下载的，下载过程十分曲折，显示关注公众号，在网页百度网盘下载，在安装百度网盘。。。。。。麻烦的要死。百度的东西，你懂的，下载完我就卸载了。
{% endnote %}
