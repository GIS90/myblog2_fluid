---
title: Bash脚本之自动解压zip
index_img: /img_index/index/20220924-001.jpeg
categories:
  - [Linux]
tags: [Linux, Bash, Bash脚本系列]
top: false
abbrlink: 42651
date: 2022-09-24 09:00:12
updated: 2022-09-24 09:00:12
desc: Bash脚本之自动解压zip
keywords: Bash, 解压zip
---



{% label primary@Bash %} {% label info@Bash脚本系列 %}

{% raw %}
<div class="post_cus_note">Bash脚本系列</div>
{% endraw %}

<!--more-->
<hr />


记录日常服务器运维用到的Bash脚本。


#### 1、简介

每天新接收新的zip文件，并且zip文件压缩的时候是直接集成了文件，没有把文件放在一个目录里面在进行压缩。

#### 2、源码

```
#!/usr/bin/bash


LDIR=/home/mingliang.gao/newdir
cd $LDIR
files=$(ls | grep ".zip")

for file in $files
do
  echo $file
  NAME=`echo "$file" | cut -d'.' -f1`
  #EXTENSION=`echo "$file" | cut -d'.' -f2`
  if [ ! -e "${NAME}" ]; then
    unzip ${LDIR}/${file} -d ${LDIR}/${NAME}
  fi
done
```

#### 3、配置

LDIR：*.zip文件所在的目录

#### 4、系列

其他Bash系列请点击tag标签进行查看。


<font size=6.5 color='red'>Bash脚本系列，持续更新中。。。。。。</font>
