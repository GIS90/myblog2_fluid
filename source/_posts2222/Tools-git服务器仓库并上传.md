---
title: Git工具服务器建立仓库并上传
index_img: /img_index/index/20201030-001.png
categories:
  - [工具集]
tags: [git]
abbrlink: 40662
date: 2020-10-30 20:53:28
updated: 2020-10-30 20:53:28
desc: 在远程服务器上面建立属于远程存储git仓库，把代码从本机上传到服务器，方便进行发布
keywords: git, 服务器, 版本管理, 代码发布, linux
---

{% note primary %}
项目要经常修改bug、上线等操作，如果是公司的话会有统一的gitlab、jenkins等工具，但是对于我个人来说，我自己的博客、开源项目都需要发不到服务器上。
目前，本人经常用2种方式：
* 把项目tar打包压缩，通过scp上传到服务器，解压，通过supervisor管理项目。
* 在服务器上做一个git仓库，用来实时更新。
本篇主要讲述第二种git仓库方式，<font color='red' size=4.5>完美使用</font>！！！。
{% endnote %}


{% label default@git %} {% label primary@代码发布 %} {% label success@版本管理 %}

<!--more-->
<hr />


#### 1、ssh 用户@服务器IP

#### 2、切换目录

```
cd 目标目录
# git目录
mkdir -p /home/mingliang.gao/git
# 项目目录
mkdir -p /home/mingliang.gao/projects
```
这里要mkdir2个目录，一个用于存放git仓库，一个用于真实存放项目文件。

#### 3、初始化仓库

```
cd 仓库目录
git init --bare 项目名.git
```

#### 4、编辑post-receive钩子

```
cd 项目名.git/hooks
vim post-receive
```
post-receive内容如下：
```
#!/bin/sh
git --work-tree=/home/mingliang.gao/projects/blog --git-dir=/home/mingliang.gao/git/blog.git checkout -f
```
--work-tree：项目实际目录
--git-dir：仓库目录

#### 5、post-receive权限

```
chmod 777 post-receive
```

#### 6、push

到这里就可以对项目进行git操作，push项目到服务器了。
