---
title: ATOM安装插件的方式
index_img: /img_index/index/20211024-001.jpg
tags: [Atom, IDE]
categories:
  - [工具集]
abbrlink: 10421
date: 2021-10-24 22:05:31
updated: 2021-10-24 22:05:31
desc: ATOM安装插件的方式，主要有IDE SETTINGS、APM、手动安装
keywords: ATOM, 插件, IDE, SETTINGS, APM, 手动安装
---

{% note warning %}
之前写过一片关于ATOM的快捷键以及相关的一些基础插件，今日再把安装插件的方式记录一下，还是比较简单的。
{% endnote %}


<!--more-->
<hr />

总结一下插件的安装方式，主要有3种。

#### 1、 IDE安装

1.打开Atom->Perferences，快捷键：command+,
2.找到左侧的install菜单，输入你想要安装的Packages插件名/Theme名
![](atom_install1.png)

#### 2、APM命令安装

1.打开terminal控制台
2.执行以下命令：
```
# 查询是否有apm命令
which apm

# 安装
apm install

# 查看插件安装列表
apm list
```
具体的命令查询help，其实大部分安装包的命令都一样，npm、pip等等等，安装install、列表list、卸载uninstall/remove。
![](atom_install2.png)

#### 3、手动安装

上面2种安装解决了大部分安装插件的问题，如果在库中没有，需要手动git项目进行安装，步骤如下：
1.进入到atom的插件目录（cd ~/.atom/package），再通过git clone克隆仓库到本地
2.cd到插件的目录，执行apm install命令进行插件的安装

<font size=6.5 color='red'>最近有些懈怠了，写写文章、学学习，让自己充实起来，人生需要不断的前进。</font>
