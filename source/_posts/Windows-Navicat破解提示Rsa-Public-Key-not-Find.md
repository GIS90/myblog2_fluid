---
title: Navicat破解提示Rsa Public Key not Find..
index_img: /img_index/index/2026-10-20-001.png
tags:
  - Navicat
categories:
  - Windows
abbrlink: 49647
date: 2026-10-20 09:13:56
updated: 2026-10-20 09:13:56
---

Navicat是支持各种数据库的连接管理工具，备份、模型等日常功能应有尽有，总之夯就完了。


<!--more-->
<hr />

### 问题
对于Navicat Premium15进行破解的时候，点击Patch.exe生成密钥的时候，需要在安装时输入Rsa Public Key，否则会提示Rsa Public Key not Find。

### 解决方案

原因：如果是在激活 Navicat 15 时看到这个提示，原因通常是操作顺序反了：先打开了 Navicat，再点注册机的 patch，导致破解用的公钥文件没有被正确生成或替换。

正确操作顺序：
1、彻底关闭 Navicat（确保进程完全退出），找到 Navicat 的安装目录。
![](1.png)
2、以管理员身份运行注册机（Keygen/Patch 程序）。
![](2.png)
3、在注册机里点击 Patch，并选中 Navicat 的安装目录下的 navicat.exe 文件。
![](3.png)
4、Patch 成功后，再打开 Navicat 进行激活操作。


### 下载地址

通过网盘分享的文件：Navicat Premium.zip
链接: https://pan.baidu.com/s/1bCzBGnjN_bTxlwmJDVQDag?pwd=aay9 提取码: aay9	