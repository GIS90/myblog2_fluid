---
title: Tools-Caddy前后端分离项目配置
index_img: /img_index/index/20251113-001.png
tags: [Caddy, Web服务器]
categories: [工具集]
abbrlink: 7767
date: 2025-11-13 18:16:05
updated: 2025-11-13 18:16:05
---


{% note primary %}

### 简介
本人开发了一个前后端分离的项目，网站地址：https://2l.pygo2.top/
打个广告，一直都是用nginx进行项目部署，但是发现浏览器会自动转为https，直接通过腾讯域名获取了免费的一年https服务，现在到期了，每次访问都需要处理，很麻烦。
于是，用caddy工具去替代nginx，记录一下配置。


{% endnote %}


<!--more-->
<hr />

### 项目介绍

| 项目 | 开发语言 | 版本 |
| :---: | :---: | :---: |
| 前端 | VUE | 2 |
| 后台 | Python | 3.7.X |


### 配置

配置2个域名分别指向不同的项目，域名配置在腾讯云域名管理上进行配置。
```
# 前端
2l.pygo2.top {
    root * /opt/www/open2lui/dist
    try_files {path} {path}/ /index.html
    file_server

    encode gzip

    log {
        output file /var/log/caddy/2l-ui.log {
            roll_size 30MB
            roll_keep 10
            roll_keep_for 720h
        }
        format json
    }
}


# 后端
api.pygo2.top {
    reverse_proxy http://127.0.0.1:9999 {
        header_up Host {upstream_hostport}
        header_up X-Real-IP {remote}
        # header_up X-Forwarded-For {remote}
        header_up X-Forwarded-Proto {scheme}
    }

    log {
        output file /var/log/caddy/2l-api.log {
            roll_size 30MB
            roll_keep 10
            roll_keep_for 720h
        }
        format json
    }
}
```
#### 前端

前端是用过npm打包后的文件，所以都是静态文件，直接用file_server就行。

#### 后台

通过域名指向，把接收到的请求直接通过reverse_proxy转发到云服务器后台API服务。


### 注意点

#### http配置
不管是前端、还是后台项目，仔细检查配置文件，把http请求服务一律改成https。


#### caddy后台reverse_proxy配置

转发到云服务器请求的时候直接用http就行了。


#### 前端配置后台请求

