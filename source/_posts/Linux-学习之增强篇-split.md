---
title: Linux学习之增强篇-split
index_img: /img_index/index/20250831-001.png
categories:
  - [Linux]
tags: [Linux, Linux增强篇]
abbrlink: 23818
date: 2025-08-31 21:09:02
updated: 2025-08-31 21:09:02
---


### 1、简介

{% note info %}
split命令，就是字面意思，把文件进行切割。
{% endnote %}

{% label default@Linux %}  {% label success@高级教程系列 %} {% label info@日志切割 %} 

<!--more-->

<hr />

单位使用的SVN进行项目版本管理，最近一次备份提交的时候出现以下的错误：
```
Adding  (bin)  08源码\JAVA\2025\aespas.20250729.tar.gz
Transmitting file data .svn: E730054: Commit failed (details follow):
svn: E730054: Error running context: 远程主机强迫关闭了一个现有的连接。
```
应该是文件太大了，足足有300+MB，于是第一个想法就是把大文件切割了再进行上传。分割的方式有很多种，可以用在线网站，也可以使用脚本、命令，本人推荐Linux的自带split命令，非常强大。

### 2、推荐指数
```
🌟🌟🌟
```


### 3、常用参数

|   选项	|   含义	|  示例  |
|:------:|:------:|:------:|
|-b, --bytes=SIZE	|按大小分割|	-b 100M|
|-l, --lines=NUMBER	|按行数分割|	-l 1000|
|-d, --numeric-suffixes	|使用数字后缀|	-d|
|-a, --suffix-length=N	|后缀长度|	-a 3|
|--verbose	|显示进度信息	|--verbose|
|--help|	显示帮助信息	|--help|


### 4、分割使用方法

#### 4.1、按大小分割文件

重点参数：-b
file.part.为生成文件的前缀
```
# 将 file.tar.gz 分割成每个 100MB 的文件
split -b 100M file.tar.gz file.part.

# 使用数字后缀
split -d -b 100M file.tar.gz file.part.
```
|    单位	|  含义	|   示例   |
|:-----:|:-----:|:-----:|
|K	|Kilobyte (1000 bytes)	|-b 100K|
|M	|Megabyte (1000 KB)|	-b 500M|
|G	|Gigabyte (1000 MB)|	-b 1G|
|KB	|Kibibyte (1024 bytes)|	-b 100KB|
|MB|	Mebibyte (1024 KB)|	-b 500MB|

#### 4.2、按行数分割

重点参数：-l
```
# 将 log.txt 每 1000 行分割成一个文件
split -l 1000 log.txt log_segment.

# 分割并使用数字后缀
split -d -l 5000 access.log access.part_
```

#### 4.3、输出文件名前缀

```
# 默认文件名称，使用字母后缀（默认：aa, ab, ac...）
split -b 100M document.pdf doc.part.

# 使用数字后缀 (00, 01, 02...)
split -d -b 100M largefile.dat backup.

# 指定后缀长度（3位数字）
split -a 3 -d -b 1G data.bin data.part.
```
### 5、合并使用方法

#### 5.1、合并分割的文件

```bash
# 方法 1：使用 cat 命令（推荐）
cat file.part.* > restored_file.tar.gz

# 方法 2：明确指定文件顺序
cat file.part.00 file.part.01 file.part.02 > restored_file.tar.gz
```

#### 5.1、验证文件完整性
```bash
# 比较原始文件和合并文件的 MD5
md5sum original_file.tar.gz restored_file.tar.gz

# 比较 SHA256
sha256sum original_file.tar.gz restored_file.tar.gz
```

### 6、结束语

<div style="font-style: italic;font-size: 25px;font-weight:800;color: red;"> 求知的心将会指引前进的路～～～～～～</div>
