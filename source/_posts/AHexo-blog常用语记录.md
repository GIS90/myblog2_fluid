---
title: Blog常用markdown语法模板记录
index_img: /img_index/index/20191023-001.jpeg
categories:
  - [Hexo]
desc: blog常用语记录，主要用于markdown语法
abbrlink: 57737
date: 2018-10-23 19:18:28
updated: 2019-10-23 19:18:28
tags: [Hexo]
keywords: hexo, Hexo, 常用语, markdown
---


<font size=6.5 color='red'>主要用于记录我写博文常用的markdown语句，持续更新中。。。。。。</font>



<!-- more -->

<hr />

### Label

{% label primary@primary %} {% label success@success %} {% label danger@danger %} {% label warning@warning %} {% label info@info %} 


### 便签

{% note primary %}
success
{% endnote %}

{% note secondary %}
success
{% endnote %}

{% note success %}
success
{% endnote %}

{% note danger %}
success
{% endnote %}

{% note warning %}
success
{% endnote %}

{% note info %}
success
{% endnote %}

{% note light %}
success
{% endnote %}


### 按钮

> 效果

{% btn url, text, title %}

> 代码

```
Markdown：
{% btn url, text, title %}

HTML：
<a class="btn" href="url" title="title">text</a>
```


### 特殊语

{% raw %}
<div class="post_cus_note">========123木头人----------</div>
{% endraw %}

> 表格

| id  | name |   Version    |
|:---:|:----:|:------------:|
|  1  |  Os  | MacOS10.15.6 |
|  2  | IDE  |   PyCharm    |

> 效果

<font color="red" size="5">***特殊语！！！***</font>
<font size="4" color="red">***Schemes***</font>

> 代码

```
<font color="red" size="5">***特殊语！！！***</font>
<font size="4" color="red">***Schemes***</font>
```

<!--more-->

<hr />

### 超链接

<a href="/articles/31494/" target="_blank" class="block_project_a">outage-常用复杂sql记录</a>

### 配置文件

```
Hexo: blog/_config.yml
Next: blog/theme/next/_config.yml
md template: blog/scaffolds/post.md
```

### 图片

> 效果

<img src="article_hadoop.jpg" style="border:1.5px solid blue" width="750" alt="图片说明"/>

> 代码

```
<img src="article_hadoop.jpg" style="border:1.5px solid blue" width="750" alt="图片说明"/>
```

> 并排
```
{% gi total n1-n2-... %}
  ![](url)
  ![](url)
  ![](url)
  ![](url)
  ![](url)
{% endgi %}
```

### linux常用md结构
{% code %}

### 简介

### 介绍

### 正文

#### 格式

#### 参数说明

#### 常用命令

### 说明

### 补充

### 学习

{% endcode %}
