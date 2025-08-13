---
title: 新MacOS系统更新导致微信截图问题
index_img: /img_index/index/20191028-001.png
categories:
  - [工具集]
tags: [MacOS]
abbrlink: 17077
date: 2019-10-28 10:05:29
updated: 2019-10-28 10:05:29
desc: 新MacOS系统更新后导致微信截取不了屏幕的问题
keywords: MacOS, 微信, 截图
---
#### 1、问题背景


{% note primary %}
Mac更新了新版的OS之后，发现微信截图除了能获取到微信界面信息，其他的信息都获取不到了。微信设置了截图的快捷键，平时也用习惯了，不能快捷键去操作常用的操作，岂不<font color='red' size=6.5>蓝瘦、香菇</font>。
{% endnote %}

<!--more-->
<hr />

问题出现的很简单，更新系统前可以使用，更新系统后不能使用了，说明还是系统的问题。直接针对于系统进行排查即可，而且MacOS基于unix开发的，所以跟权限差不多相关。

快捷键： shift + command + 4

#### 2、解决方案

> 系统偏好设置 -> 安全与隐私

![](shezhi.png)

> 隐私 -> 找到屏幕录制 -> 勾选微信

![](anquanyuyinsi.png)

#### 3、温馨提示

<font color='red' size=6.5>解决问题要有思路，而且要善于思考。</font>
