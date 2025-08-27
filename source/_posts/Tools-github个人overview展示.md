---
title: Github个人Overview展示
index_img: /img_index/index/20211226-001.jpg
categories:
  - [工具集]
tags: [git]
abbrlink: 61451
date: 2021-12-26 19:47:04
updated: 2021-12-26 19:47:04
desc: Github个人Overview展示
keywords: Github, Overview, repo,
---

{% note primary %}
前几天看到一个项目，在README.md上有彩色标签、Github个人统计信息，挺有趣的，然后在Github上查到有一个专门的项目用来做这个，学习记录一下。
{% endnote %}



<!--more-->
<hr/>

先来看下原本的Github Overview试图：
![](overview_src.png)

接下来，对其进行优化。

> 创建新的Repository

首先，在Github中创建一个新的repository，Owner与Repository name保持一致。比如你的Owner为ABC，那么就创建一个ABC仓库。
选择Public、Add a README file选项。

> 编辑README.md文件

创建好了之后，直接对仓库README.md文件进行编辑，这里简单对标签、GitHub Readme Stats进行举例，详情请查阅官网，本人也是探索阶段。

> README.md标签

打开标签牌官网：https://shields.io/

![](shields.io.png)

最简单标签：
- label：标签体的前半部分，比如：Development Language
- message：标签体的后半部分，比如：Python
- color：选择一个颜色，也可以输入十六进制代码

示例：
```
![](https://img.shields.io/badge/Development%20Language-Python-FF0000)
```
![标签牌](https://img.shields.io/badge/Development%20Language-Python-FF0000)


> GitHub统计

官网：https://github.com/anuraghazra/github-readme-stats/blob/master/docs/readme_cn.md
示例：
```
![GitHub stats](https://github-readme-stats.vercel.app/api?username=GIS90&theme=highcontrast&show_icons=true&hide=contribs,prs&count_private=true)
```


官网介绍的很明白，请自行查阅。

看下最终效果：
![](overview_tar.png)


> 学习参考

README标签：https://shields.io/
GitHub统计：https://github.com/anuraghazra/github-readme-stats/blob/master/docs/readme_cn.md

<font color="red" size="6.5">Enjoy the good life everyday！</font>
