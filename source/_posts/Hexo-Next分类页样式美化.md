---
title: Hexo+Next分类页样式美化
index_img: /img_index/index/20190505-001.png
categories:
  - [Hexo]
tags: [Hexo, Hexo美化]
top: false
abbrlink: 15926
date: 2019-05-05 23:01:43
updated: 2019-05-05 23:01:43
desc: 关于hexo+next搭建的博客，自定义分类页的样式
keywords: hexo, next, Hexo, 美化, 插件, 博客, blog, 分类页, 样式, css, 自定义
---
### 问题简述
{% note success %}
自动生成的分类页实在是ugly，既然身为程序猿，前端不行那就自己搞，起码自己看起来舒服一些。
{% endnote %}



<!-- more -->
<hr/>

### 解决方案
美化方法跟自定义样式差不多，不做详细介绍了，不知道的小伙伴可以查看之前的文章。这里主要给大家分享一下我的样式，有问题的小伙伴可以留言我，<font color="red" size="5">***一起交流，一起学习，一起进步！！！***</font>。

css代码：
```
// 分类&&标签 页面样式
.post-block.page {
    margin-top: 40px;
}

// 分类页面page
.category-all-page {
    box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.5);
    background-color: #797D7F;
    padding: 20px 30px 60px 30px;
    border-radius: 25px 25px 25px 25px;
}
.category-all-title {
    font-family: Impact;
    font-size: 24px;
    color: aqua;
}
.category-list {
    overflow: auto;
}
.category-list li {
    height: 30px;
    float: left;
    border-right: 3px solid #222;
    padding: 0 20px;
}
.category-all ul li {
    list-style: none!important;
}
.category-list li:last-child {
    border-right: none;
}
.category-list li a {
    font-size: 16px;
    text-decoration: none;
    color: chartreuse;
    font-family: Helvetica, Verdana, sans-serif;
    // text-transform: uppercase;
    -webkit-transition: all 0.5s ease;
    -moz-transition: all 0.5s ease;
    -o-transition: all 0.5s ease;
    -ms-transition: all 0.5s ease;
    transition: all 0.5s ease;
}
.category-list li a:hover {
    color: black;
}
.category-list li.active a {
    font-weight: bold;
    color: black;
}
```
