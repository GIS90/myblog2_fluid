---
title: Linux服务上安装Win字体
index_img: /img_index/index/20210125-001.jpg
categories:
  - [服务器]
tags: [Linux, CkEditor]
abbrlink: 34016
date: 2021-01-25 10:51:39
updated: 2021-01-25 22:51:39
desc: Linux服务器上添加字体库
keywords: Linux, 字体库, 字体, 宋体, mkfontscale, mkfontdir, fc-list, fc-cache
---

{% note warning %}
在开源项目上集成了CKEditor富文本编辑器，但是发现面板上字体没有宋体、微软雅黑等Win字体，web服务大部分应用都是win系统，在CKEditor上config.js添加了字体选择，但是没有效果，需要在Linux服务器添加所需要的字体。
{% endnote %}

{% label primary@Linux %} {% label info@CKEditor %}

<!--more-->
<hr />

这里介绍2种配置，均可以。

> 系统环境

Contos7.5

> 基础操作

拷贝Win系统的字体传到Linux服务器上，放在/usr/share/fonts/chinese目录下，fonts目录下是没有chinese文件夹的，需要root用户新建并赋予755权限。
Win系统的字体库位置：C:/Windows/Fonts
Linux涉及命令：su、scp、mkdir、chmod

{% raw %}
<div class="post_cus_note">================第一种================</div>
{% endraw %}
```
cd /usr/share/fonts/chinese
sudo mkfontscale
sudo mkfontdir
sudo fc-cache –fv
```
执行以上命令之后，在执行***fc-list***查看字体列表，查看中文字体：***fc-list :lang=zh***。

> mkfontscale && mkfontdir 区别

***man***查看二者命令：
```
mkfontscale - create an index of scalable font files for X
mkfontdir - create an index of X font files in a directory
```
都是在字体库目录建立索引文件的命令，而且内容也是相同的。
![](cmd.png)

{% raw %}
<div class="post_cus_note">================第二种================</div>
{% endraw %}

- 用编辑器打开/etc/fonts/fonts.conf，通常会有如下内容:
```
<dir>/usr/share/fonts</dir>
<dir>/usr/share/fonts/chinese</dir>
<dir>/usr/share/X11/fonts/Type1</dir> <dir>/usr/share/X11/fonts/TTF</dir> <dir>/usr/local/share/fonts</dir>
<dir prefix="xdg">fonts</dir>
<!-- the following element will be removed in the future -->
 <dir>~/.fonts</dir>
```
把中文字体库文件放到其中一个目录即可或者把自定义的目录路径加到fonts.conf。

- 刷新字体库
fc-cache -fv

- 查看已安装字体
fc-list :lang=zh

<font color="red" size="5">***纵使生活再艰难，也不要放弃，微信面对，致自己！！！***</font>
