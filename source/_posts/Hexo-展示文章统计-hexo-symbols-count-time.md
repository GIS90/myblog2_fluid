---
title: hexo展示文章统计-hexo-symbols-count-time
desc: hexo展示文章统计-hexo-symbols-count-time
index_img: /img_index/index/20181124-001.jpg
categories:
  - [Hexo]
tags: [Hexo, Hexo插件]
comments: false
keywords: hexo, next, Hexo, 美化, 插件, 博客, blog, hexo-symbols-count-time, 统计, 字数统计, 阅读时长
abbrlink: 16195
date: 2018-11-24 16:52:30
updated: 2019-12-21 21:52:30
---
### 问题简述

{% note success %}
next主题的blog添加文章统计功能【hexo-symbols-count-time】
{% endnote %}

<!-- more -->

<hr />

***My blog***采用next主题，想在发布的文章中加上通统计相关的展示，google许多文章，都是关于配置hexo-wordcount、LeanCloud等，发现配置之后，仍然没有效果。身为一名程序猿，怎么可能有解决不的bug，下面是我的解决方案，官方配置。

### 版本信息

| id |     name     | version |
| :-: | :----------: | :-----: |
| 1 |     Hexo     | v3.8.0 |
| 2 | NextT.Pisces | v7.0.1 |

### 解决方案

去查next主题的_config.yml文件，一点点看。终于在347行发现了***# Post wordcount display settings***这一行说明，继续阅读发现hexo-symbols-count-time这个插件，立马google，官方解释：
![hexo-symbols-count-time](hexo-symbols-count-time.png)
大致的意思是统计文章的符号数量以及阅读时间，比hexo-reading-time更美观，比hexo-worcount统计的更快，而且还没有依赖。这是非常爽的啊，只需要按配置一下，OK了啊。

#### 安装

```
npm install hexo-symbols-count-time --save
```

#### blog配置文件

⁍ next找到_config.yml的配置，添加以下内容：

```
# 文章统计
symbols_count_time:
 #文章内是否显示
  symbols: true
  time: true
 # 网页底部是否显示
  total_symbols: true
  total_time: true
```

#### theme配置文件

⁍ next找到_config.yml的配置，搜索关键字：hexo-symbols-count-time

```
symbols_count_time:
  separated_meta: true
  item_text_post: true
  item_text_total: false
  awl: 4
  wpm: 275
```

#### 说明

AWL — Average Word Length (chars count in word). Default: 4. You can check this here.
    CN ≈ 2
    EN ≈ 5
    RU ≈ 6
WPM — Words Per Minute. Default: 275. You can check this here.
    Slow ≈ 200
    Normal ≈ 275
    Fast ≈ 350

### 相关文章

***hexo-wordcount：***https://www.jianshu.com/p/baea8c95e39b
***hexo-symbols-count-time：***https://github.com/theme-next/hexo-symbols-count-time

### 个人观点

* 搞开发的英语看的能力起码得有基础，不然解决bug，看官方api的时候很费劲
* 搞一个东西的时候，先从本身查找能否解决问题，如果不能在考虑从外部引用解决问题
