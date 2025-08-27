---
title: tags与categories重构的教训
index_img: /img_index/index/20190423-002.jpg
categories:
  - [Hexo]
tags: [Hexo]
top: false
desc: tags与categories重构，导致一些问题以及修复方案
keywords: hexo, next, Hexo, 美化, 插件, 博客, blog, tags, categories, 404, not found, 页面, 多tags, 多categories
abbrlink: 43145
date: 2019-04-23 15:31:47
updated: 2019-04-23 15:31:47
---

{% note success %}
<font size="4" color="red">**问题简述**</font>
博客已经托管在github有一段时间了，有天同事想看看，url发送给同事，但是随便点开了一个文章，居然**404**，逗我。。。。。。why？
{% endnote %}

![](article_404.png)

<!--more-->

<hr />

启动本机上的server，找到那篇404的文章，直接把***协议+ip+port***替换到我本机，找到那篇资源文章，我本机可以啊，问题出在哪？仔细查看了一下2个资源地址：
```
本机：http://127.0.0.1:8888/Hexo/Hexo%E6%90%9C%E7%B4%A2hexo-generator-searchdb/
github：https://gis90.github.io/hexo/Hexo%E6%90%9C%E7%B4%A2hexo-generator-searchdb/
```
居然是Hexo与hexo资源路径的问题，立马查看本机blog/public目录的Hexo文件夹是大写，找到github托管的repositories，居然真的不一样，我记得我***hexo g -d***过了啊，立马又执行了1次，发现github上还是小写的hexo文件目录。

![日了。。。。。](404_ri.jpeg)

直接***hexo deploy***，在blog根目录会自动生成一个.deploy_git隐藏文件，这个文件目就是github托管上传用的，查看里面内容，再次确认是大写。没办法，只能删除github上现在托管的网站，又重新新建了一个deploy了一下，这下因为资源路径的导致的404问题解决了。

想起前几天我刚把每篇文章的tag、categories的内容都改成首字母大写的英文了，这是资源路径发生改变的根本，在我本地生成正确的资源路径，deploy到github是错误的，为何？还在查资料寻求原因中。

既然说到了tag && categories，那么就多说点，有些博文不止一个tags、categories，记录一下它们的用法。

> tag
> > 定义：tag标签
> > 用法：
> > {% code %}
> > tags:
> >   - 123
> >   - 456
> > tags: [123, 456]
> > {% endcode %}
> > 多标签写法，这2种都是一样的效果，用哪个都可以，建议使用列表[]式，直观清晰。


> categories
> > 定义：文章分类
> > - 用法一：
> > {% code %}
> > categories: 123
> > {% endcode %}
> > 这是默认的写法，给文章添加一个分类。
> > - 用法二：
> > {% code %}
> > categories: [123, 456]
> > {% endcode %}
> > 文章位于自分类下。
> > - 用法三：
> > {% code %}
> > categories:
> >   - 123
> >   - 456
> > {% endcode %}
> > 这会将文章分类到123/456子分类目录下。
> > - 用法四：
> > {% code %}
> > categories:
> >   - [123]
> >   - [456]
> >   - [123, 789]
> > {% endcode %}
> > 多标签写法，文章被分类到123、456以及123的自分类789这3个分类下面，官方指定写法。

{% note info %}
参考：
> tags：https://github.com/hexojs/hexo/issues/320
> categories：https://github.com/MOxFIVE/hexo-theme-yelee/issues/4
{% endnote%}
