---
title: Bootstrap的动态icon
index_img: /img_index/index/20210801-001.png
categories:
  - [前端开发]
tags: [Bootstrap]
abbrlink: 63837
date: 2021-08-01 10:07:44
updated: 2021-08-01 10:07:44
desc:
keywords: Bootstrap, icon, 动态, fa
---


{% note primary %}
Bootstrap图标使用的是Font Awesome，加上基本的class可以产生动画效果，让页面看起来更加酷炫。
{% endnote %}

{% label primary@Bootstrap %} {% label info@icon %}

<!--more-->
<hr />

实现代码：
```
<div class="fa-5x">
    <i class="fa fa-spinner fa-spin"></i>
    <i class="fa fa-refresh fa-spin"></i>
    <i class="fa fa-cog fa-spin"></i>
    <i class="fa fa-spinner fa-pulse"></i>
    <i class="fa fa-refresh fa-pulse"></i>
    <i class="fa fa-cog fa-pulse"></i>
    <i class="fa fa-spinner fa-rotate-90"></i>
    <i class="fa fa-refresh fa-rotate-90"></i>
    <i class="fa fa-cog fa-rotate-90"></i>
</div>
```
- fa-spin：让icon图标不停的动态旋转
- fa-pulse：图标以八步为周期进行旋转，比较适合表示刷新、加载等功能的图标，如.fa-spinner、.fa-refresh、.fa-cog等
- fa-rotate-xx：图标以某个角度进行选择
- fa-flip-horizontal：水平翻转
- fa-flip-vertical：垂直翻转
- fa-3x：图标大小的设置，有fa-x、fa-2x、fa-3x，想要多大有多大，使用默认大小fa-x居多
