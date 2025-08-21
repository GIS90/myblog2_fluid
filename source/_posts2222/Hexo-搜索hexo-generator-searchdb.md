---
title: Hexo搜索插件hexo-generator-searchdb
index_img: /img_index/index/20190412-001.jpg
desc: Hexo搜索hexo-generator-searchdb
categories:
  - [Hexo]
tags: [Hexo, Hexo插件]
keywords: hexo, next, Hexo, 美化, 插件, 博客, blog, hexo-generator-searchdb, 站内搜索, 菜单栏
abbrlink: 21643
date: 2019-04-12 19:09:21
updated: 2019-04-12 19:09:21
---


#### 1、背景
{% note success %}
博客运行了很久，也写了不少文章，有的时候需要回去翻自己写过的文章找点东西，比较麻烦，而且也看了别人的博客好多都有一个🔍**搜索**功能，针对于我使用的***Hexo+Next***版本也增加一个搜索功能。但是，在实现的过程中遇到了一些坑，想必也会有人跟我一样，于是写下来与大家分享。
{% endnote %}

<!--more-->
<hr />

#### 2、版本信息

| id  | name | version |  remark  |
|:---:|:----:|:-------:|:--------:|
|  1  | Hexo | v3.8.0  | 系统版本 |
|  2  | Next | v7.0.1  | 主题版本 |

#### 3、安装hexo-generator-searchdb

博客项目的根目录直接执行：
```
npm install hexo-generator-searchdb --save
```

#### 4、配置
打开Next主题配置文件：/blog/theme/next/_config.yml，搜索<font size="4" color="red">***local_search***</font>，更改代码如下：
```
local_search:
  enable: true
  trigger: auto
  top_n_per_article: 1
  unescape: false
```

#### 5、hexo g && hexo s
完成hexo-generator-searchdb的插件安装与配置之后，重启服务。重启之后查看blog首页sidebar多出一个**🔍搜索**功能，点进去查看，如果能实现搜索功能，那么恭喜你，你的功能已经没问题。
![](search_sidebar.png)

#### 6、遇到问题

点击搜索，我发现我的搜索modal一直在转圈圈，<font size="5" color="red">根本停不下来</font>。
![](search_promble.png)

#### 7、解决问题

网上找了许久解决办法，一般都是结束安装、配置，[hexosearch](https://www.sqlsec.com/2017/12/hexosearch.html)这篇文章提供了一个解决思路与方法，我在这里大致讲述一下：
- 直接访问博客地址search.xml：http://127.0.0.1:8888/search.xml
- 发现错误提示
![](search_error.png)
- 打开控制台查看原因
![](search_console.png)
- 查看提示错误提示以及console查看具体位置，具体错误的会有小红点（<font size="4" color="red">**•••••**</font>），找到具体对应的md文章，清除特殊字符。
- 全部特殊字符处理完之后，重新：***hexo g && hexo s***。

不出意外，你的**🔍搜索**已经可以用了，有问题可以在sidebar留言板留言共同交流分享。

#### 8、学习

Next local search：https://github.com/iissnan/hexo-theme-next/pull/694
hexo-generator-searchdb官方：https://github.com/theme-next/hexo-generator-searchdb
hexosearch：https://www.sqlsec.com/2017/12/hexosearch.html

#### 9、感谢

<font size="5" color="red">感谢Hexo、Next、以及解决我问题的博主：国光</font>，推荐一下国光博主的博客：https://www.sqlsec.com/
