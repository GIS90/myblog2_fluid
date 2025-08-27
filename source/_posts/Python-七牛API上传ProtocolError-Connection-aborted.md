---
title: 七牛API上传文件发送ProtocolError-Connection-aborted错误
index_img: /img_index/index/20230325-001.png
categories:
  - [Python]
tags: [Python]
abbrlink: 43878
date: 2023-03-25 14:59:54
updated: 2023-03-25 14:59:54
desc: 七牛API上传文件发送ProtocolError-Connection-aborted错误
keywords: Python, 七牛, ProtocolError, Connection, aborted
---


{% label default@Python %}

#### 背景
{% note primary %}
自己开发的<font size=5.5 color='red'>OPENTOOL-Z</font>系统在PDF转WORD功能文件上传上发送上传失败，后台查看日志发送了ProtocolError('Connection aborted.', timeout('timed out'))的错误，记录以下解决方法。

{% endnote %}


<!--more-->
<hr />


开发完PDF转WORD功能之后测试了几个小文件的都是1.2M大小的PDF文件，上传与转换都没有问题，突然有一天同事找我让我转换一个6.3M大小的文件，上传了好几次都是显示上传失败。

#### 报错

于是，登录到服务器，查看存储的文件，发现服务器本地存储成功了，是上传到七牛云对象存储失败，打开日志错误如下：
![](error.jpg)
```
[WARNING] Retrying (Retry(total=0, connect=None, read=None, redirect=None, status=None)) after connection broken by 'ProtocolError('Connection aborted.', timeout('timed out'))': /buckets/open2lstore/objects/MjAyMzAzMjUv6KGl5YWF5Y2P6K6uMjAyMjA5MTXmiavmj4_ku7YucGRm/uploads/641e55eff67b72651c9d93c0region09cn-east-2/1
None
_ResponseInfo__response:None, exception:HTTPConnectionPool(host='upload-cn-east-2.qiniup.com', port=80): Max retries exceeded with url: /buckets/open2lstore/objects/MjAyMzAzMjUv6KGl5YWF5Y2P6K6uMjAyMjA5MTXmiavmj4_ku7YucGRm/uploads/641e55eff67b72651c9d93c0region09cn-east-2/1 (Caused by ProtocolError('Connection aborted.', timeout('timed out'))), status_code:-1, text_body:None, req_id:None, x_log:None, error:HTTPConnectionPool(host='upload-cn-east-2.qiniup.com', port=80): Max retries exceeded with url: /buckets/open2lstore/objects/MjAyMzAzMjUv6KGl5YWF5Y2P6K6uMjAyMjA5MTXmiavmj4_ku7YucGRm/uploads/641e55eff67b72651c9d93c0region09cn-east-2/1 (Caused by ProtocolError('Connection aborted.', timeout('timed out'))
```

#### 解决方案

一开始我本地开启项目上传10M大小的文件都没有问题，大概拆到了是服务器带宽的问题，因为服务器我买的是腾讯云新用户99一年的服务器，配置都是最低的，于是安装了nload命令去观察上传带宽，果然上传的带宽限制了文件上传的问题。
我在官网提了相关错误的报错工单，官网工程师给出的解决方案：
![](jiejue.png)

就是修改官网Pyhton SDK的参数，修改后的参数：
```
_config = {
    'default_zone': zone.Zone(),
    'default_rs_host': RS_HOST,
    'default_rsf_host': RSF_HOST,
    'default_api_host': API_HOST,
    'default_uc_host': UC_HOST,
    'connection_timeout': 120,  # 链接超时为时间为30s
    'connection_retries': 3,  # 链接重试次数为3次
    'connection_pool': 10,  # 链接池个数为10
    'default_upload_threshold': 2 * _BLOCK_SIZE  # put_file上传方式的临界默认值
}
```
把connection_timeout连接时间由默认的30秒修改为120秒。


#### 其他

在解决过程中学到了一个新的实时测带宽的命令nload。
> nload

![](nload.png)

除了nload，也可以使用speedtest-cli进行网速测评。
> speedtest-cli

![](speedtest-cli.jpg)


#### 总结

虽然服务器配置较低，如果可以利用起来，还是可以干很多事情的，而且在自己搞的时候，遇到了别人没有遇到过的问题，总结起来，慢慢会的就多了。

<font size=6.5 color='red'>坚持学习，总会有收获。。。。。。</font>
