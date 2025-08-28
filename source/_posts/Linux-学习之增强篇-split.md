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
split，就是字面意思，把文件进行切割。
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
于是

### 2、推荐指数
```
🌟🌟🌟
```

### 3、使用方法
* ***awk -F '分隔符' '相关命令' 文件1,文件2,...***
* ***cat 文件 | awk -F '分隔符' '相关命令'***

> 分隔符

awk会依据-F后的参数‘’对数据进行切割，常见的分隔符***空格(‘ ’)***、***冒号(‘:’)***、***关键字***，都为英文字符。

> 内置变量

|   变量   | 说明                                    | 指数 |
|:--------:|:--------------------------------------- | :----: |
|    $0    | 当前记录内容（作为单个变量）              |   🌟   |
|  $1~$n   | 当前记录的第n个字段，字段间由-F参数分隔 |   🌟🌟🌟   |
|    NF    | 当前记录中的字段个数，就是有多少列      |   🌟   |
|    NR    | 已经读出的记录数，就是行号，从1开始     |   🌟🌟   |
|   FNR    | 当前总记录数                        |   🌟   |
|    FS    | 输入字段分隔符 默认是空格             |   🌟    |
|   OFS    | 输出字段分隔符 默认也是空格           |      |
|    RS    | 输入的记录他隔符默 认为换行符         |      |
|   ORS    | 输出的记录分隔符，默认为换行符         |      |
| FILENAME | 当前输入文件的名字                   |   🌟   |

> 相关命令

通用方式：***'BEGIN {print "name, count"}  {print $1","$7;print $0;} END {print "end------"}'***
- print：打印输出
- printf：格式化打印输出(print format)
- begin：第一行，主要用来打印头部
- end：末尾一行，主要用来打印结束标识
- 中间部分：可执行多行语句，每个语句以；结束
***命令需要在单引号‘’内，有begin、end、中间3部门，begin && end可省略***

### 4、基础使用
    环境：MacOs
    文件：/etc/passwd

> 显示所有用户名

命令：***cat /etc/passwd |awk  -F ':'  '{print $1}'***
小解：
- $1：分割的后的第1个变量
![etc_1](etc_1.png)

> 用户与对应的shell

命令：***cat /etc/passwd |awk  -F ':'  'BEGIN {print "start========="}  {print $1","$7} END {print "end------"}'***
小解：
- $7：分割的后的第7个变量
- begin：输出的第一行标识
- end：输出的结束标识(图片未显示完全)
![etc_2](etc_2.png)

> 统计/etc/passwd:文件名，行号，列数

命令：***cat /etc/passwd |awk  -F ':'  'BEGIN {print "start=========================="} {print "filename:" FILENAME ",linenumber:" NR ",columns:" NF ",linecontent:"$0}'***
小解：
- $0：当前行的记录
- FILENAME：文件名称
- NR：当前记录的行数
- NF：当前记录中的字段个数
![etc_3](etc_3.png)

> 统计用户数量

命令：***cat /etc/passwd |awk  -F ':'  'BEGIN {print "start=========================="} {print "filename:" FILENAME ",linenumber:" NR ",columns:" NF ",linecontent:"$0}'***
小解：
- $0：当前行的记录
- FILENAME：文件名称
- NR：当前记录的行数
- NF：当前记录中的字段个数
![etc_3](etc_3.png)

命令：***cat /etc/passwd | awk '{count++;} END{print "user count is ", count}'***
![etc_4](etc_4.png)



### 5、实战解析nginx的access.log
直接引用本人现在负责的项目日志进行分析，***reality && reliable ！！！***
> demo1

![isapi_dmeo](isapi_dmeo.png)

统计不同类型require请求的次数：
cat isapi-access.log | awk -F "require=|&" '{c[$2]+=1;} END{for (i in c) printf"%d\t%s\n",c[i], i;}' | sort -nr | head -n 20
统计ip的请求次数：
cat isapi-access.log | awk -F " " '{c[$1]+=1;} END{for (i in c) printf"%d\t%s\n",c[i], i;}' | sort -nr | head -n 20

> demo2

![papproval_dmeo](papproval_dmeo.png)

统计不同类型请求次数
awk '{c[$7]+=1;} END{for (i in c) printf"%d\t%s\n",c[i], i;}' psapproval-access.log | sort -nr
统计ip的请求次数：
awk '{ips[$1]+=1;} END{for (ip in ips) printf("%d\t%s\n",ips[ip], ip);}' /home/q/var/log/psapproval-access.log | sort -nr

> 命令小解

    cat：查看文件
    awk：分析命令
    sort：排序
    head：查看文件，但是打开的是文件的前XX行

### 6、相关连接
- ***isapi_access.log***：
https://github.com/GIS90/project_data_ref/blob/master/isapi_access.log

- ***psapproval_access.log***：
https://github.com/GIS90/project_data_ref/blob/master/psapproval_access.log

### 7、追加说明

本人对awk也是初步了解，如果有不对的地方，欢迎大家留言，一起share。。。。。。待续
