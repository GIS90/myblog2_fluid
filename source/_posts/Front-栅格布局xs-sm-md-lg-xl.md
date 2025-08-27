---
title: 前端栅格布局xs sm md lg xl详解
index_img: /img_index/index/20221008-001.jpg
categories:
  - [前端开发]
tags: [HTML, CSS]
desc: 前端栅格布局xs sm md lg xl详解
keywords: '前端, 栅格,布局, xs, sm, md, lg, xl'
abbrlink: 6149
date: 2022-10-08 23:12:19
updated: 2022-10-08 23:12:19
---


{% note primary %}
在写VUE的时候用到了Layout布局组件，el-col里面有多个属性是控制大小的，一直都是搞后台的，css基本是小白，查阅资料记录一下学习的内容。
{% endnote %}



<!--more-->
<hr />

看下官网实例，响应式设计：
```
<el-row :gutter="10">
  <el-col :xs="8" :sm="6" :md="4" :lg="3" :xl="1"><div class="grid-content bg-purple"></div></el-col>
  <el-col :xs="4" :sm="6" :md="8" :lg="9" :xl="11"><div class="grid-content bg-purple-light"></div></el-col>
  <el-col :xs="4" :sm="6" :md="8" :lg="9" :xl="11"><div class="grid-content bg-purple"></div></el-col>
  <el-col :xs="8" :sm="6" :md="4" :lg="3" :xl="1"><div class="grid-content bg-purple-light"></div></el-col>
</el-row>
```
里面包含了xs sm md lg xl属性来控制宽度，对这几个值进行详解：
- xl【extra large】：屏幕 >= 1920px，超大显示器
- lg【large】：1200px <= 屏幕 <= 1920px，台式1920*1080显示器
- md【middle】：992px<= 屏幕 <= 1200px，适合笔记本
- sm【small】：768px <= 屏幕 <= 992px，适合平板
- xs【extra small】：屏幕<=768，手机端屏幕

> 优点

如果屏幕的大小发生改变，系统会根据设置的大小进行分别显示，感觉挺好用的。

> 测试

正常分辨率

![](1.png)

更改分辨率

![](2.png)
