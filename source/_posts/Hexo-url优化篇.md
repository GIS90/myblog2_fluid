---
title: Hexo-url优化篇
index_img: /img_index/index/20190509-001.png
categories:
  - [Hexo]
tags: [Hexo, Hexo插件, Hexo美化]
top: false
abbrlink: 44359
date: 2019-05-09 08:29:50
updated: 2019-05-09 08:29:50
desc: 关于博客的url优化相关的处理以及讲解
keywords: hexo, next, Hexo, 美化, 插件, 博客, blog, url, permalink
---

### 1、背景
{% note success %}
曾几何时，有莫有觉得自己的博文url过于繁琐，而且博文名称包含中文，发送链接变成unicode编码，很不友好。那好，**hexo-abbrlink**插件解决url带来的难题。
{% endnote %}

<!--more-->
<hr />

### 2、版本信息

| id  | name | version |  remark  |
|:---:|:----:|:-------:|:--------:|
|  1  | Hexo | v3.8.0  | 系统版本 |
|  2  | Next | v7.0.1  | 主题版本 |

### 3、url格式

正常的url格式默认是：**:year/:month/:day/:title/**，resource地址过长，如果想改变，需要在Hexo的配置去更改。
打开blog/_config.yml文件，搜索<font size="4" color="red">***permalink***</font>，更改配置如下：
```
# permalink: :year/:month/:day/:title/  # default版
# permalink: :category/:id/             # id版
# permalink: :category/:title/          # name版
```
category是分类，id对应的是博文默认生成的id，如果不想使用插件的同学，建议使用**id版**，配置好之后***hexo g && hexo s***重启server即可。具体的url配置请查看[官方说明](https://hexo.io/zh-cn/docs/permalinks)。

到这里，已经完成了url的配置，如果不想继续优化url的同学，完成上述配置就可以了，下面将继续介绍使用插件优化url。

### 4、安装hexo-abbrlink

博客项目的根目录直接执行：
```
npm install hexo-abbrlink --save
```

### 5、Hexo配置

还是刚才的配置文件，把<font size="4" color="red">***permalink***</font>改成以下配置：
```
permalink: articles/:abbrlink/
```

### 6、Next配置
打开Next主题配置文件：/blog/theme/next/_config.yml，新增配置如下：
```
# post url
abbrlink:
  alg: crc32  #support crc16(default) and crc32
  rep: hex    #support dec(default) and hex
```
{% note info %}
<font size="4">**参数**</font>
alg -- Algorithm (currently support crc16 and crc32, which crc16 is default)
rep -- Represent (the generated link could be presented in hex or dec value)
<font size="4">**例子**</font>
> crc16 & hex

https://post.zz173.com/posts/66c8.html

> crc16 & dec

https://post.zz173.com/posts/65535.html
> crc32 & hex

https://post.zz173.com/posts/8ddf18fb.html

> crc32 & dec

https://post.zz173.com/posts/1690090958.html

{% endnote %}

###7、 hexo clean && hexo g

这一步是必须操作，***hexo clean***清除public站点文件，***hexo g***重新生成站点文件。生成之后，打开一个.md博文文件，你会发现，在顶部的定义部分新增了一个**abbrlink**属性，后面是文件的url资源路径。
```
abbrlink: 44359
```
在老的文件***hexo g***会自动新增这个属性，在建立新的文件也会自动新增这个属性，属性值根据上面的**alg && rep**配置的算法规则生成。

### 8、hexo s

重启服务。重启之后回到home主页，点开一个博文查看url，不出意外url已发生改变，没有改变的同学查看下2个配置文件处，是否配置对了，别搞混了，不要都配置到一个文件中。

### 9、学习

Hexo permalinks：https://hexo.io/zh-cn/docs/permalinks
hexo-abbrlink：https://github.com/rozbo/hexo-abbrlink
