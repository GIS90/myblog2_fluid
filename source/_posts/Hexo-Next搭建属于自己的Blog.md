---
title: Hexo+Next搭建属于自己的Blog
index_img: /img_index/index/20181022-001.jpg
desc: 基于Hexo+Next搭建属于自己的Blog，基础环境搭建学习
categories:
  - [Hexo]
tags: [Hexo]
keywords: hexo, next, Hexo, 博客, blog
abbrlink: 59864
date: 2018-10-22 23:03:42
updated: 2018-10-22 23:03:42
---

### 1、简介
{% note success %}
Hexo + Next + MarkDown 完美打造属于Blog，只属于你自己的哦！！！
{% endnote %}

***<font color="#dd0000" size="5">FROM ZERO TO ALL</font>***

### 2、前言

从GIS跳跃到Python开发之后，一直梦想着有自己的一个blog，前不久机缘巧合解决问题的时候，发现作者的blog网站正是我心中向往的那种样式。没错，就是Hexo + Next（footer有版权声明，易发现）。说搞就搞，折腾了好几天，终于实现了，虽然路程有点长，但是看着自己亲手搭建的blog，心中还是无比喜悦。既然有了属于自己的一片天地，那就将知识***share***出来。

<!--more-->
<hr />

### 3、环境

| id  |     name     | version | remark   |
|:---:|:------------:|:-------:| :--------: |
|  1  |     system     | MacOs  | 系统版本 |

### 4、正文

    三步走。

#### 一步：hexo安装

首先查看自己电脑是否安装nodejs、git，如果已经安装请忽略nodejs、git的安装步骤，查看方式（控制台）：
```
node -v     查看nodejs版本
which git   查看环境变量是否有无git命令
which brew  查看机子是否含有brew命令
```
> brew

- 简述：MacOs开发必备工具，类似于pip等。
- 功能：Mac软件管理工具。
- 安装：
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
> nodejs

- 简述：安装nodejs（nodejs不了解的可以去百度一下，哈哈），安装完nodejs，npm会随nodejs一起安装。
- 功能：npm包管理工具。
- 安装：
```
brew install node
```
> git

- 简述：安装git工具，主要用户项目的发布、管理等。
- 功能：项目管理工具。
- 安装
```
brew install git
```

> hexo

- 简述：Hexo是一个快速、简洁且高效的博客框架，基于nodejs运行，利用Markdown解析文章。
- 功能：博客框架。
- 安装
```
npm install -g hexo-cli
```

#### 二步：启动server

> 新建myblog文件夹

在指定文件夹中执行初始化命令，命令会根据传的参数进行初始化工作。
```
cd ~/github/myblog
hexo init PyGo
```
![hexo_next_init](hexo_next_init.png)

> 生产静态文件

生产web网站的静态网页。
```
hexo g
```
> 启动server服务

启动server。
```
hexo s
```
![hexo_next_server](hexo_next_server.gif)
浏览器中访问 http://localhost:4000/ ，就可以看到基于Hexo的默认主题的原型。
![hexo_next_server](hexo_next_server.png)

#### 三步：更换主题Next

> 下载主题

在blog目录，执行以下命令下载Next源文件。
```
git clone https://github.com/iissnan/hexo-theme-next themes/next
```
> 更改配置

下载完成后，打开blog目录下的_config.yml文件，搜索关键：***theme***，更改代码如下：
```
theme: next
```

> 重启服务
停止服务（ctrl+c），重新开启服务，刷新页面。
![hexo_next](hexo_next.png)

### 5、链接

- Hexo官网：https://hexo.io/zh-cn/docs/index.html
- 主题地址：https://hexo.io/themes/
- Next官方：https://theme-next.org/

在hexo主题官方地址有很多theme，根据自己的喜欢选取，个人觉得***Next***主题比较整洁，黑白色style上比较简单。

### 6、注意

在*** Hexo && Next ***的配置和设置文件中，一定要保留一个***英文空格***，没留空格会导致出问题。

### 7、感谢
    感谢Hexo开发者，感谢Next主题开发者，让我拥有了自己第一个Blog。

希望自己可以在技术上继续走下去，为自己打***CALL***，后续也会继续写出Hexo命令的使用教程以及Next优化，欢迎大家***Share***。
