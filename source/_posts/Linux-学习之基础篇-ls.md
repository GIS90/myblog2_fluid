---
title: Linux学习之基础篇-ls
index_img: /img_index/index/20170515-001.jpg
desc: 记录Linux命令学习基础篇之ls，列举文件list file
categories:
  - [Linux]
tags: [Linux, Linux基础篇]
keywords: linux, ls, 服务器, 命令, shell, bash
abbrlink: 58978
date: 2017-05-15 16:51:42
updated: 2017-05-15 16:51:42
---

最近看了“每日一命令”博客，也学习了一些Linux命令基础，结合自己的实践总结一下，加深学习，记录一下ls命令，用于列表文件，全名：<font size=6.5 color='red'>list file</font>。

<!--more-->
<hr />

### 1、简介
{% note warning %}
学习ls的用法【ls - list directory contents】
{% endnote %}

### 2、介绍
linux最常用的2个命令：***ls && cd***。
ls是打印当前目录/指定目录文件list的命令，全拼list。为何是文件呢，正所谓在***<font color="#dd0000" size="4">linux中一切皆文件</font>***。 ls命令不仅可以查看linux文件的类型、大小、权限等各种信息，而且最主要的是日常使用频率最高的命令之一。

### 3、正文

#### 3.1、格式

```
ls [参数][目录]
```

#### 3.2、参数说明

选用了一些自认为常用的参数进行说明，详细的请自行***man ls***进行查看。翻译过程中，使用了[fanyi.baidu.com](https://fanyi.baidu.com/)，如果不对，请见谅！

> -a, –all

Eng：do not ignore entries starting with。
Chi：列出目录下的所有文件，包括隐含文件（.开头）。


> -d, --directory

Eng：list directories themselves, not their contents。
Chi：列出目录本身，对于列出目录包含的哪有目录很有用，后面常用命令会讲到。

> -F, --classify
-p, --indicator-style=slash

Eng：append / indicator to directories。
Chi：2个参数是一样的效果，放到一起说了，都是在目录后面加个“/”作为结束标识。

> -h, --human-readable

Eng：with -l, print sizes in human readable format (e.g., 1K 234M 2G)。
Chi：与l参数一起使用，就是文件大小变得可读性更高。

>  -i, --inode

Eng：print the index number of each file。
Chi：经常与l参数使用，列出文件的存储节点id，一般情况用不上，但是有必要知晓一下。

> -l
-o


Eng：use a long listing format。
Chi：常用的参数没有之一，经常与h一起使用，列举文件的类型、大小、权限等各种信息。

> -L

Eng： when showing file information for a symbolic link, show information for the file the link references rather than for the link itself。
Chi：显示链接文件直接引用文件的相关信息。

> -m

Eng：fill width with a comma separated list of entries。
Chi：用逗号分隔的列表

> -n, --numeric-uid-gid

Eng：like -l, but list numeric user and group IDs。
Chi：与l参数显示的内容一致，但是用户、组用相应的id代替。

> -r, --reverse

Eng：reverse order while sorting。
Chi：反序。

> -R, --recursive

Eng：list subdirectories recursively。
Chi：是否进行递归。

>  -S

Eng：sort by file size。
Chi：按文件大小进行排序。

> -t
-c

Eng：sort by modification time, newest first
Chi：按文件的修改时间进行排序（最新优先排在前面）。

#### 3.3、常用命令

1. 列举文件详情，-i参数加不加都行
```
ll -hi
```
![](article_linux_ls_ll.png)
total 48：所列出内容的磁盘占用空间总和值，单位为kb。
第一列：drwxr-xr-x，一共10个字符，分别解释一下。第一位用来代表文件的类型：“d”代表目录，“-”代表普通文件，2-4位rwx代表该文件的属主的权限，5-7位r-x代表文件用户主所在用户组的其他用户的权限，8-10位r-x代表其他用户的权限。其中r-读、w-写、x-执行。
第二列：未知（网上查资料说如果是文件就是1，如果是目录就是目录下的文件，试验过不准确）。
第三列：文件所属的用户。
第四列：文件所属的用户组。
第五列：文件的大小，如果文件是目录则代表该目录的大小（不包括目录下的子目录和文件的大小）。
第六列：该文件最近修改或者查看的时间。
第七列：文件名称。

2. 列举文件中包含的目录/文件
    - 列举当前文件目录包含文件目录，这个命令有个弊端，就是只能列举当前文件目录下【本人暂时还不能切换目录列举，知道的同学可以留言给我】
    ```
    ll -d */
    ```
    - 获取指定目录下的目录，可递归，把“^d”换成“^-”就可以获取对应的文件，-R代表是否启用递归操作。
    ```
    ll /home/q/psapproval -R | grep "^d"
    ```
    - 计算当前目录下的文件数和目录数，基于上面的命令加了***wc -l***，想要把子目录也计算进行，加上-R参数
    ```
     ls -l * |grep "^d"|wc -l
    ```
    - 列举文件以及目录，用“/”区别
    ```
    ll /home/q/psapproval -F
    ll /home/q/psapproval -p
    ```
3. 按时间进行排序，最新的文件在最下面
```
ll -rth
```
4. 文件按从小到大进行排序，最大的在下面，想要生序去掉-r参数即可。
```
ll -Shr
```
5. help
```
man ls
ls --help
```


### 4、说明

{% note primary %}
1. 示例中的命令我都加了指定的目录，如果是当前目录，省略即可。
2. 本文只讲解了ls常用的参数，了解更多请***man ls***查看。
{% endnote %}

### 5、补充

{% note info %}
文件类型
{% endnote %}

- “-”普通文件
- “d”目录
- “l”链接文件
- “b”块设备文件
- “c”字符设备文件
- “s”套接字文件，主要用于sock通信用的文件

{% note info %}
文件权限
{% endnote %}

- r：读取的权限等于4，用r表示
- w：写入的权限等于2，用w表示
- x：执行的权限等于1，用x表示

{% note info%}
ll命令
{% endnote%}

控制台直接执行：***alias ll='ls -l --color=auto'***

### 6、学习

ls常用命令://www.tecmint.com/15-basic-ls-command-examples-in-linux/
ll详解：https://blog.csdn.net/LEON1741/article/details/82386520
