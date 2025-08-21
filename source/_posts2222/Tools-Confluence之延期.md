---
title: WIKI之Confluence授权的延期
index_img: /img_index/index/20200416-001.png
categories:
  - [工具集]
tags:
  [WIKI, 运维工具]
desc: 关于试用的confluence wiki进行延期授权
keywords: 'wiki, Confluence, 授权, 延期, 工具, 授权码'
abbrlink: 39160
date: 2020-04-16 09:14:11
updated: 2020-04-16 09:14:11
---

#### 简述

{% raw %}
<div class="post_cus_note">关于WIKI Confluence的延期授权</div>
{% endraw %}
{% label default@WIKI %} {% label primary@Confluence %} {% label success@授权 %} {% label info@延期 %}

<!--more-->
<hr />

#### 解决方案


> 点击My Altassian，进入到https://my.atlassian.com/官网。

![](1.png)

> 点击New Trial License，创建新的Trial License。

![](2.png)

> 选择相关对应的产品：confluence。

![](3.png)

> 使用admin登录confluence服务器。

![](4.png)

> 查看confluence的服务器ID，在<font color='red' size=6.0>设置->一般配置->管理→授权细节</font>，复制服务ID信息。

![](5.png)

> 在填写New Trial License页面的相关信息，选择confluence产品，输入服务器ID，在create license，会自动生成授权码（复制）。

![](6.png)

> 回到confluence授权细节页面，把复制的授权码粘贴在授权细节中的授权码，点击保存，完成延期使用。

![](7.png)
