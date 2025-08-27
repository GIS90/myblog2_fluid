---
title: GIT修改默认分支master改为main
index_img: /img_index/index/20211208-001.png
categories:
  - [工具集]
tags: [git]
abbrlink: 28122
date: 2021-12-08 21:03:57
updated: 2021-12-08 21:03:57
desc: GIT修改默认分支master改为main
keywords: git, 服务器, 版本管理, 代码发布, linux
---

{% note info %}
最近git初始化项目，本地都是默认master分支，毕竟发生了“黑人维权”，不管怎么说，github官网发布了消息：GitHub 官方发布信息称，从2020年10月1日起，在 Github平台上创建的所有新的源代码仓库将默认命名为 "main" ，不再是原先的 "master"。
我的本地居然还没改，于是，找了下解决办法，配置一下就ok了。
{% endnote %}


{% label info@git %} {% label primary@默认分支 %} {% label success@github %}

<!--more-->
<hr/>

> 配置

执行一下的命令，就可以设置默认初始化仓库的时候为main仓库，把这个配置到全局的配置文件中。
```
git config --global init.defaultBranch main
```

> 查看

```
$ git config --global --list

init.defaultbranch=main
```
