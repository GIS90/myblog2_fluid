---
title: Linux云服务器环境初始化
index_img: /img_index/index/20201128-001.png
categories:
  - [服务器]
tags: [Linux, git, Mysql]
abbrlink: 49695
date: 2020-11-28 08:43:34
updated: 2020-11-28 08:43:34
desc: 腾讯云服务器的环境初始化，包含用户、数据库、VIM、Supervisor等安装
keywords: Linux, git, MariaDB, VIM, Supervisor, swap, nginx
---

{% label success@Linux %} {% label info@SSH %}  {% label danger@Swap %} 
<hr />
{% label warning@Supervisor %} {% label primary@Nginx %}{% label default@git %}

### 1、前言
{% note info %}
新购买的腾讯云服务器需要在上面进行环境的一些处理以及软件安装，这里我介绍一下我的服务器初始化过程。
{% endnote %}


<!--more-->
<hr />

![](20201128-001.png)

### 2、系统

CentOS Linux release 7.5.1804 (Core)

Linux查看系统版本：
```
cat /etc/*release*


CentOS Stream release 8
NAME="CentOS Stream"
VERSION="8"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="8"
PLATFORM_ID="platform:el8"
PRETTY_NAME="CentOS Stream 8"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:8"
HOME_URL="https://centos.org/"
BUG_REPORT_URL="https://bugzilla.redhat.com/"
REDHAT_SUPPORT_PRODUCT="Red Hat Enterprise Linux 8"
REDHAT_SUPPORT_PRODUCT_VERSION="CentOS Stream"
CentOS Stream release 8
CentOS Stream release 8
cpe:/o:centos:centos:8
```

### 3、初始化

#### 3.1、用户

> 添加用户与组

```
# 新建用户
useradd mingliang.gao
# 新建组
groupadd opsdev
# 用户设置密码
passwd mingliang.gao
# 用户添加工作组
usermod -G opsdev mingliang.gao
```

> 配置用户root权限

root用户vim /etc/sudoers
```
root	ALL=(ALL) 	ALL
mingliang.gao    ALL=(ALL)    ALL
```

> 其他

```
删除用户：userdel 用户名
删除组：groupdel 组名
```

#### 3.2、环境编码

修改服务器系统环境编码，执行vim /etc/profile，加入一下内容：
```
# 编码
export LC_ALL=en_US.UTF-8
export LANG=en_US.UTF-8
```

查看环境编码：
```
[root@VM-4-7-centos mingliang.gao]# echo $LANG
en_US.UTF-8

[root@VM-4-7-centos mingliang.gao]# locale
locale: Cannot set LC_CTYPE to default locale: No such file or directory
locale: Cannot set LC_ALL to default locale: No such file or directory
LANG=en_US.UTF-8
LC_CTYPE=zh_CN.UTF-8
LC_NUMERIC="en_US.UTF-8"
LC_TIME="en_US.UTF-8"
LC_COLLATE="en_US.UTF-8"
LC_MONETARY="en_US.UTF-8"
LC_MESSAGES="en_US.UTF-8"
LC_PAPER="en_US.UTF-8"
LC_NAME="en_US.UTF-8"
LC_ADDRESS="en_US.UTF-8"
LC_TELEPHONE="en_US.UTF-8"
LC_MEASUREMENT="en_US.UTF-8"
LC_IDENTIFICATION="en_US.UTF-8"
LC_ALL=
```

#### 3.3、VIM

新的云服务器初始化环境只有VI，但是编辑的时候VIM很好用，需要安装。

> 安装

```
yum -y install vim
```

> 配置

vi /etc/vimrc，打开vimrc配置问题添加一下2行简单配置，如果详细配置请百度。
```
set nu          " 设置显示行号
set showmode    " 设置在命令行界面最下面显示当前模式等
```

#### 3.4、SSH

用于免密码连接登录。
```
ssh-copy-id 用户@服务器IP
输入用户登录密码
```
详情：<a href="/articles/1431/" target="_blank" class="block_project_a">SSH之免密码登录</a>

#### 3.5、SWAP

有些人可能对swap可能陌生，简单说下SWAP可以理解成虚拟内存，当服务器物理内存不足时，拿出部分硬盘指定空间当swap分区（虚拟成内存）使用，从而解决内存容量不足的情况。
可以先free一下看下是否又swap，如果想配置请用root用户操作。

> 生成

```
dd if=/dev/zero of=/var/swap bs=1024 count=3072000
mkswap /var/swap
swapon /var/swap
free -m
```
解释一下dd，可以理解成dd获取了磁盘的一块空间，有兴趣的可以深入学习一下dd。

> Swap开机初始化

vi /etc/fstab，最后一行添加
```
swap /var/swap swap defaults 0 0
```

> 清理swap

```
swapoff /var/swap
```

> Swap活跃使用度

vim /proc/sys/vm/swappiness，范围是0～100，指数越大使用的活跃度越大，建议30-50即可。

#### 3.6、GIT

> 查看

```
which git
```

> 安装

```
yum -y install git
```

> 配置

下面是全局配置，也可以在单独的git项目中单独配置属于项目的git配置。
```
# 用户名
git config --global user.name "mingliang.gao"
# 邮箱
git config --global user.email "gaoming971366@163.com"

# 查看配置
git config --list
```

#### 3.7、MariaDB

<a href="/articles/3296/" target="_blank" class="block_project_a">MariaDB安装与配置</a>

#### 3.8、Miniconda

> 下载

这里介绍2种方式，第一种就是直接去官网下载：https://docs.conda.io/projects/miniconda/en/latest/
第二种方式直接在服务器上执行wget：
```
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```

> 安装

```
base Miniconda3-latest-Linux-x86_64.sh
```
- Do you accept the license terms? [yes|no] 输入yes接受许可条款
- 配置安装路径，默认安装到根目录下的，可自定义路径（建议/usr/local/miniconda）
- 等待安装后，出现Do you wish the installer to initialize Miniconda3，如果输入yes会对~/.bashrc文件添加一下内容
```
# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
__conda_setup="$('/usr/local/miniconda/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/usr/local/miniconda/etc/profile.d/conda.sh" ]; then
        . "/usr/local/miniconda/etc/profile.d/conda.sh"
    else
        export PATH="/usr/local/miniconda/bin:$PATH"
    fi
fi
unset __conda_setup
# <<< conda initialize <<<
```

> 配置

生效：
```
source ~/.bashrc
```
禁止用户登录后自动激活base环境：
```
conda config --set auto_activate_base
```

具体conda命令请参考学习官网或者baidu。

#### 3.9、Gunicorn

> 安装

在python项目运行环境中会进行安装，单独安装命令：
```
pip install gunicorn
```

> 日志配置

python项目需要用到，提前建好，用来存放日志，root用户执行。
```
mkdir -p /var/log/gunicorn
```

#### 3.10、Supervisor

> 安装

```
yum install -y supervisor

# 启动服务
systemctl start supervisord

# 开机自启动：
systemctl enable supervisord
```

> 查看下进程

```
ps -ef | grep supervisord
```
系统默认的启用命令是：/usr/bin/supervisord -c /etc/supervisord.conf

> 自定义配置

切换root执行。
```
echo_supervisord_conf > /etc/supervisord.d/supervisord.conf
cd /etc/supervisord.d
mkdir include
mkdir -p /var/log/supervisord
```
echo_supervisord_conf是配置文件写入，把项目配置文件与是sp的配置文件放在一起便于管理，建立include存放项目的对应配置文件，以下是本人的配置文件：
```
[unix_http_server]
file=/var/run/supervisor.sock   ;;UNIX socket 文件，supervisorctl会使用其与supervisord通信
chmod=0777                     ;;socket文件的mode，默认是0700，改成0777: chmod 777 /var/run/supervisor.sock
;chown=nobody:nogroup           ;;socket文件的owner，格式：uid:gid

;[inet_http_server]          ;;web管理界面
;port=0.0.0.0:10001          ;;Web管理后台运行的IP和端口
;username=mingliang.gao      ;;登录管理后台的用户名
;password=971366             ;;登录管理后台的密码

[supervisorctl]
serverurl=unix:///var/run/supervisor.sock   ;;通过UNIX socket连接supervisord，路径与unix_http_server部分的file一致
;serverurl=http://0.0.0.0:10001              ;;通过HTTP的方式连接supervisord

[rpcinterface:supervisor]
supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface

[supervisord]
logfile=/var/log/supervisord/supervisord.log    ;;日志文件
logfile_maxbytes=50MB               ;;日志文件大小，超出会rotate，默认 50MB。如果设成0，表示不限制大小
logfile_backups=10                  ;;日志文件保留备份数量默认10，设为0表示不备份
loglevel=info                       ;;日志级别，默认info，其它: debug,warn,trace
pidfile=/var/run/supervisord.pid    ;;pid 文件
nodaemon=false                      ;;是否在前台启动，默认是false，即以 daemon 的方式启动
minfds=1024                         ;;可以打开的文件描述符的最小值，默认 1024
minprocs=200                        ;;可以打开的进程数的最小值，默认 200

[include]
files = /etc/supervisord.d/include/*.conf
```
启动命令：/usr/bin/supervisord -c /etc/supervisord.d/supervisord.conf
-c为指定配置文件。

这样启动的supervisor如果重启需要杀死进程：
```
ps -ef | grep supervisord
kill 进程ID
```
为了方便我把2个命令合并到一起：
```
ps -ef | grep supervisord | grep -v grep | awk -F " " '{print $2}' | xargs kill
```
直接执行上面这句话就可以直接杀死进行，这是个通用杀死指定进程的命令，只需要更换grep supervisord需要查询过滤的进程关键字。

> 其他

贴一下自己的supervisord项目配置文件。
```
[program:open2lisapi]
directory=/home/mingliang.gao/projects/open2lisapi
command=/home/mingliang.gao/projects/open2lisapi/.venv/bin/gunicorn -c /home/mingliang.gao/projects/open2lisapi/etc/prod/gunicorn.conf wsgi:app   ;; ERROR (no such file)，把gunicorn启动命令写全，绝对路径
autostart=True                          ;; 是否开机自动启动
autorestart=True                        ;; 是否挂了自动重启
startretries = 3                        ;; 启动失败自动重试次数，默认是 3
redirect_stderr=True                    ;; 是否把stderr定向到stdout
stopasgroup=True
;;user=mingliang.gao                    ;;用哪个用户启动进程，默认是root
priority=999                            ;;进程启动优先级，默认999，值小的优先启动
stdout_logfile_maxbytes=20MB            ;;stdout 日志文件大小，默认50MB
stdout_logfile_backups = 20             ;;stdout 日志文件备份数，默认是10
stdout_logfile=/var/log/supervisord/supervisor_open2lisapi.log
```

#### 3.11、Nginx

> 检查gcc环境

gcc编译器。
```
# 检查gcc环境
gcc -v

# 安装
yum -y install gcc
```

> 安装Nginx

root用户或者sudo命令。
```
# 查看版本
yum list | grep nginx

# 安装
yum -y install nginx
```
安装后执行whereis Nginx
- 执行目录：/usr/sbin/nginx
- 模块所在目录：/usr/lib64/nginx
- 配置所在目录：/etc/nginx/
- 默认站点目录：/usr/share/nginx/html

> 启动

```
# 启动/停止/重启
systemctl start/stop/restart nginx.service
# 开启启动
systemctl enable nginx.service
# 重新加载配置
systemctl reload nginx.service
# 查看状态
systemctl status nginx.service
```
> 查看进程

```
ps -ef | grep nginx
```

> 开放端口

```
# 添加端口
firewall-cmd --zone=public --add-port=80/tcp --permanent

# 重加载或者重启
firewall-cmd --reload
systemctl restart firewalld

# 查看
firewall-cmd --list-all
```
这里根据自己的项目需求，对外开放端口。

> 配置

下面是我的nginx配置，仅供参考：
```
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

user root;
worker_processes 2;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    # use epoll
    worker_connections 1024;
}

# sendfile on;
# tcp_nopush on;

# keepalive_timeout 65;

# gzip压缩功能设置
# gzip on;
# gzip_min_length 1k;
# gzip_buffers 4 16k;
# gzip_http_version 1.0;
# gzip_comp_level 6;
# gzip_types text/html text/plain text/css text/javascript application/json application/javascript application/x-javascript application/xml;
# gzip_vary on;

# http_proxy 设置
# client_max_body_size 10m;
# client_body_buffer_size 128k;
# proxy_connect_timeout 75;
# proxy_send_timeout 75;
# proxy_read_timeout 75;
# proxy_buffer_size 4k;
# proxy_buffers 4 32k;
# proxy_busy_buffers_size 64k;
# proxy_temp_file_write_size 64k;
# proxy_temp_path /usr/local/nginx/proxy_temp 1 2;

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf;

# Settings for a TLS enabled server.
#
#    server {
#        listen       443 ssl http2 default_server;
#        listen       [::]:443 ssl http2 default_server;
#        server_name  _;
#        root         /usr/share/nginx/html;
#
#        ssl_certificate "/etc/pki/nginx/server.crt";
#        ssl_certificate_key "/etc/pki/nginx/private/server.key";
#        ssl_session_cache shared:SSL:1m;
#        ssl_session_timeout  10m;
#        ssl_ciphers HIGH:!aNULL:!MD5;
#        ssl_prefer_server_ciphers on;
#
#        # Load configuration files for the default server block.
#        include /etc/nginx/default.d/*.conf;
#
#        location / {
#        }
#
#        error_page 404 /404.html;
#            location = /40x.html {
#        }
#
#        error_page 500 502 503 504 /50x.html;
#            location = /50x.html {
#        }
#    }

}

```
- 因为有些参数在腾讯云服务器不能识别，所以有些被注释。
- user root本人使用root用户启动，使用哪个用户就配置哪个

server配置，单独文件，存放/etc/nginx/conf.d
```
server {
    listen  80;
    server_name     _;

    access_log  /var/log/nginx/default_server.access.log  main;
    root        /home/mingliang.gao/projects/blog;

    location = / {
        # index    index.html index.htm;
        index    root.html;
    }
    location / {
        index index.jsp index.html index.htm;
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Real-PORT $remote_port;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    error_page 404 /error.html;
        location = /40x.html {
    }

    error_page 500 502 503 504 /error.html;
        location = /50x.html {
    }

}
```
- server_name：域名
- listen：端口

> 常用命令

```
# 指定配置文件
nginx -c /etc/nginx/nginx.conf

# 重新加载配置文件，执行这个可以不用重启
nginx -s reload
```

> 访问

配置完把nginx重启之后，访问IP:PORT。

> 源码安装

<a href="/articles/47364/" target="_blank" class="block_project_a">Nginx源码安装教程</a>

### 4、详细连接

- <a href="/articles/3296/" target="_blank" class="block_project_a">MariaDB安装与配置</a>
- <a href="/articles/40662/" target="_blank" class="block_project_a">Git服务器仓库并上传</a>
- Supervisor学习：https://www.cnblogs.com/xuezhigu/p/7660203.html
