---
title: Caddy学习之日志配置
index_img: /img_index/index/20251012-001.png
tags: [Caddy, Web服务器]
categories: [工具集]
abbrlink: 30081
date: 2025-10-13 20:31:53
updated: 2025-10-13 20:31:53
---


### 背景
最近发现域名的https认证到期了，访问网站总是提示不安全，于是看看有啥办法能永久解决https认证问题，前提是白嫖，哈哈哈。
经过多番查找，终于找到了Caddy神器，基于golang语言编写，高并发肯定没问题，内部又自带https认证，于是进行了自学，并且进行配置。




<!--more-->
<hr />

### 正文
关于安装自行查看官网手册或者AI，讲一下基础的配置以及遇到的日志配置坑。

#### 基础配置

```
pygo2.top, www.pygo2.top {
    root * /opt/www/blog2/public

    file_server {
        index root.html index.html
    }

    encode gzip

    log {
        output file /var/log/caddy/blog2.log {
            roll_size 30MB
            roll_keep 10
            roll_keep_for 720h
        }
        format json
    }
}
```

#### 日志配置说明

- output file /var/log/caddy/blog2.log：配置日志文件位置
- roll_size 30MB：单个日志文件达到30MB后滚动
- roll_keep 10：保留10个旧的日志文件
- roll_keep_for 720h：保留720小时
- format json：日志格式，当时试了common但是提示这个参数不可用，于是采用json格式

#### 日志配置的坑

方式为了配置控制台也同时显示日志，基于上面的配置多加了一行配置：
```
output stdout
```
在journalctl -u caddy -f查看日志，控制台正常显示，但是指定的日志文件怎么生成，经过各种问题查找，终于发现了上面的坑，去掉了之后就好了。

#### 用的命令记录

```
# 验证配置
sudo caddy validate --config /etc/caddy/Caddyfile

# 重新加载配置
sudo systemctl reload caddy

# 启动/停止/重启
sudo systemctl start/stop/restart caddy

# 彻底清理日志目录并重建
sudo rm -rf /var/log/caddy
sudo mkdir -p /var/log/caddy

# 设置正确的权限（关键步骤）
sudo chown -R caddy:caddy /var/log/caddy
sudo chmod -R 755 /var/log/caddy

# 验证权限
sudo -u caddy touch /var/log/caddy/test-permission.log
echo "权限测试成功" | sudo -u caddy tee /var/log/caddy/test-permission.log
```

### 结束语
<font size=6.5 color='red'>能力不足，需要继续学习。。。。。。</font>