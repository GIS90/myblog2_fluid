---
title: NPM install报错：code128
index_img: /img_index/index/20210107-001.png
categories:
  - [前端开发]
tags:
  - VUE
  - NPM
  - NODE
desc: NPM install报错：code128
keywords: 'VUE, NPM, NODE, 128'
abbrlink: 57475
date: 2021-01-07 23:22:37
updated: 2021-01-07 23:22:37
---

{% note success %}
用的是vue-element-admin脚手架在macos上起来了，后来在win电脑上也下载了自己写的open2lui项目，***npm install***的时候发现报了code 128的错误，记录一下。
{% endnote %}



{% label info@VUE %} {% label default@NPM %}

<!--more-->
<hr />

#### 1、问题描述

在npm install安装包的时候出现一下问题：

![](error.png)

#### 2、解决方案

看了下错误，发现是raphael.git项目git不下来，baidu了一下控制台进行git设置：
```
git config --global http.sslverify "false"
git config --global url."https://".insteadOf git://
```
执行完之后，在进行***npm install***。

#### 3、推荐

基于vue-element-admin的一套后台管理系统基础模板，简单易用。
GitHub地址：https://github.com/PanJiaChen/vue-admin-template

<font size=6.5 color='red'>坚持学习。。。。。。</font>
