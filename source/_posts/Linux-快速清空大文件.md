---
title: 快速清空大文件
index_img: /img_index/index/20220116-001.jpg
categories:
  - Linux
tags:
  - Linux
  - Linux其他篇
top: false
desc: 快速清空Linux中的大文件
keywords: 'Linux, cat, shell, echo,'
abbrlink: 27322
date: 2022-01-16 22:14:34
updated: 2022-01-16 22:14:34
---


{% label info@Linux快速清空大文件 %}

<!--more-->
<hr />

#### 1.echo

```
echo "" > filename.log
```

#### 2.cat

```
cat /dev/null > access.log
```

#### 3.dd

```
dd if=/dev/null of=access.log
```

清空的原理就是重定向，方法很多，不局限。
