---
title: WIKI之Confluence之隐藏人员菜单栏
index_img: /img_index/index/20191210-001.jpg
categories:
  - [工具集]
tags: [WIKI, 运维工具]
abbrlink: 13168
date: 2019-12-10 14:15:33
updated: 2019-12-10 14:15:33
desc: WIKI之Confluence隐藏菜单栏人员的设置
keywords: wiki, Confluence, 隐藏, 人员, 菜单栏, 工具
---
{% raw %}

<div class="post_cus_note">配置WIKI之Confluence隐藏菜单栏人员的设置</div>
{% endraw %}

<!--more-->

<hr />

#### 问题背景

在公司搭建了一个Wiki，用的是Confluence + MariaDB + Linux架构，搭建之后不管在任何用户的菜单栏都会有用户这个选项，导致不管任何用户都可以邀请、注册等操作，正常来说用户管理只有管理员才有的权限，解决前如下图：

![](article_confluence_qian.png)

<font color='red' size=6.0>解决后如下图：</font>

![](article_confluence_hou.png)

#### 解决方案

在网上查了很多，都没有实际的解决方案。于是，官网走起，在官网的`<font color='red' size=6.0>`Search`</font>`下，搜索关键字：`<font color='red' size=6.0>`hide tools menu users`</font>`，结果如下：

![](confluence_search.png)

点进去查看之后，发现给了我一个解决的灵感，具体如下：

> 登录管理员账户

> 打开设置 -> 一般配置

![](confluence_config.png)

> 找到自定义HTML

![](confluence_html.png)

> 在HEAD尾部加入一下代码

![](confluence_set.png)

> 保存后进行刷新测试

#### 具体代码

```
<script>
jQuery().ready(function() {
  jQuery("#people-directory-link").hide();
});
</script>
```

#### 官方说明

Confluence官方：https://confluence.atlassian.com/confkb/how-to-hide-the-space-tools-menu-for-certain-users-or-groups-in-confluence-816877024.html
