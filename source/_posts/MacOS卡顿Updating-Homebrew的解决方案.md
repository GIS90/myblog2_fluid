---
title: Mac卡顿Updating-Homebrew的解决方案
index_img: /img_index/index/20200911-001.png
categories:
  - 工具集
tags:
  - MacOS
desc: Mac卡顿Updating Homebrew的解决方案
keywords: 'brew, install, Updating, Homebrew, 卡顿,'
abbrlink: 62547
date: 2020-09-11 14:14:03
updated: 2020-09-11 14:14:03
---

#### 问题背景

{% note primary %}
MacOS系统，用brew进行安装，但是每次执行brew install XXXX，都会有Updating Homebrew的提示，而且还要花费很久等待的时候，<font color='red' size=6.5>是时候展现真正的技术了</font>。
{% endnote %}

<!--more-->
<hr />

#### 解决方案
归根结底就是brew开启了自动更新，关闭即可。

> 编辑配置文件

我用的是item2 + zsh。
```
vim ~/.zshrc
```
如果使用自带的shell，编辑下面文件：
```
vim ~/.bash_profile
```

> 文件结尾新增一行

```
export HOMEBREW_NO_AUTO_UPDATE=true
```
> 重新加载配置文件

```
source ~/.zshrc
source ~/.bash_profile
```
