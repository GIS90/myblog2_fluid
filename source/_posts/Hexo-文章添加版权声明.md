---
title: hexo文章添加版权声明
desc: hexo博客在站内文章添加版权声明
index_img: /img_index/index/20181109-001.jpg
categories:
  - [Hexo]
tags: [Hexo]
comments: false
keywords: hexo, next, Hexo, 美化, 插件, 博客, blog, 版权声明
abbrlink: 46750
date: 2018-11-09 20:03:24
updated: 2018-11-09 20:03:24
---
#### 问题简述

{% note success %}
想在***My blog***的文章底部添加上自己的版权声明，一是意识方便大家进行转载share，二来也是对自己写的东西有一个声明吧。废话不多说，很简单。【含样式】
{% endnote %}

<!--more-->

<hr />

![](article_banquanshengming.png)

#### 版本信息

| id |     name     | version |
| :-: | :----------: | :-----: |
| 1 |     Hexo     | v3.8.0 |
| 2 | NextT.Pisces | v7.0.1 |

#### 版权代码

打开next主题目录下/next/layout/_macro/post.swig文件，搜索***post-body***，在这个div标签结束的下一行，添加以下代码

```
<!-- 版权声明 -->
<div>
 {% if not is_index %}
    <ul class="post-copyright">
      <li class="post-copyright-author">
          <strong>本文作者：</strong>{{ theme.author }}【{{ theme.subtitle }}】
      </li>
      <li class="post-copyright-link">
        <strong>本文链接：</strong>
        <a href="{{ url_for(page.permalink) }}" title="{{ page.title }}">{{ page.permalink }}</a>
      </li>
      <li class="post-copyright-license">
        <strong>版权声明：</strong>
        本博客所有文章除特别声明外，均采用 <a href="http://creativecommons.org/licenses/by-nc-sa/3.0/cn/" rel="external nofollow" target="_blank">CC BY-NC-SA 3.0 CN</a> 许可协议。转载请注明出处！
      </li>
    </ul>
  {% endif %}
</div>
```

变量说明：

* theme.*：hexo配置文件的基础信息
* page.*：指的是本文章的一些相关信息

#### 版权样式

打开/next/source/css/_custom/custom.styl,并在里面添加如下样式代码:

```
// 版权声明样式
.post-copyright {
    margin: 2em 0 0;
    padding: 0.5em 1em;
    border-left: 8px double #ff1700;
    background-color: #EAECEE;
    list-style: none;
}
.post-copyright-link a{
    color: blue;
}
.post-copyright-link a:hover {
    color: red;
    font-size: 18px;
}
```

#### 个人声明

本人在版权声明中文章链接的都是github上的地址，如果不需要去掉https://gis90.github.io/即可。
