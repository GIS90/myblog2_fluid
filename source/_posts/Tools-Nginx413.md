---
title: 413 Request Entity Too Large
index_img: /img_index/index/20210823-001.jpeg
categories:
  - [工具集]
tags: [Nginx]
abbrlink: 60121
date: 2021-08-23 22:51:13
updated: 2021-08-23 22:51:13
desc: 上传文件提示413 Request Entity Too Large内容，配置nginx去解决问题
keywords: Nginx, 413, Request, Entity, Too, Large
---


{% note primary %}
#### 问题说明
在发布的pyhton web项目上传2M的文件，提示<font color='red' size=4.5>413 Request Entity Too Large</font>，请求实体过大也就是requests的Content-Length过大，记得当时本机开发的时候没有报过这个问题，为何上线就有了，大概率是域名、nginx那的问题。
{% endnote %}



<!--more-->
<hr />

![](article_nginx413.jpeg)


#### 解决方案

确认问题出处之后就好解决，baidu查了一下，是因为nginx的配置问题，找到云服务器nginx的配置：/etc/nginx/nginx.conf
在http配置处添加
```
client_max_body_size 100M;
```
重启nginx，搞定。
