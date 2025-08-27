---
title: Oracle数据库连接工具PLSQL快捷配置
index_img: /img_index/index/20230228-001.jpg
categories:
  - [数据库]
tags: [Oracle, PLSQL]
abbrlink: 13254
date: 2023-02-28 22:53:26
updated: 2023-02-28 22:53:26
desc: Oracle数据库连接工具PLSQL快捷配置
keywords: Oracle, 数据库, 连接工具, PLSQL, 快捷配置
---




正所谓“功先利其事，必先利其器”，把PLSQL的快捷方式设置好，在写SQL以及操作方面会大大提高效率，分享下本人PLSQL的快捷配置。


<!--more-->
<hr />





#### 显示行号

> 用户界面 > 编辑器 > 显示行号

可以显示SQL窗口的行号。
![](showline.PNG)

#### 关键词大写

> 用户界面 > 编辑器 > 关键词大小写 > 大写

这个看个人喜好，如果想把关键字都大写可以进行设置，这样写出来的SQL也比较好看，关键字都是大写并且带颜色。
![](keycase.PNG)

#### 高亮显示编辑行

> 用户界面 > 编辑器 > 高亮显示编辑行

可以高亮SQL窗口的编辑行，一下子就找到鼠标在哪，方便继续编辑SQL。
![](hightline.jpg)

#### 延时tip设置

> 用户界面 > 代码助手 > 延时

设置自动提示的时间，最小值100，不喜欢立马提示的可以值设置的打点。
![](tip.PNG)

#### 不自动提交

> 窗口类型 > SQL窗口 > 自动提交SQL

设置不自动提交，否则一下子做的危险UPDATE、DELETE等操作，想想都是泪，千万别设置自动提交。
![](nocommit.PNG)



#### SQL快捷语句

> 用户界面 > 编辑器 > 自动替换

设置之后，写SQL非常的高效，只需要把缩写写完一个空格，就可以替换设定的SQL语句或关键字。
![](autoreplace.PNG)

```
sf=SELECT * FROM
sw=SELECT * FROM  WHERE
w=WHERE
or=ORDER BY
order=ORDER BY
ij=INNER JOIN
in=INNER JOIN
inn=INNER JOIN
lj=LEFT JOIN
le=LEFT JOIN
rj=RIGHT JOIN
ri=RIGHT JOIN
drop=DROP TABLE
```

#### 快捷键配置

PLSQL自带了许多快捷键配置，但是习惯了IDE写代码的快捷键，所以对部分配置、以及未配置的功能进行了更改，快捷键配置说明【修改部分】：
**********************************************************************************************************


| 序号 |                  功能                  |       快捷键        |
|:----:|:--------------------------------------:|:-------------------:|
|  1   |      文件/打开/新建(N)/SQL窗口(S)      |     Ctrl+Alt+A      |
|  2   |           文件/关闭/关闭(C)            |     Ctrl+Alt+W      |
|  3   |           编辑/选择/缩进(I)            |         Tab         |
|  4   |         编辑/选择/取消缩进(U)          |      Shift+Tab      |
|  5   |           编辑/选择/注释(O)            |       Ctrl+/        |
|  6   |         编辑/选择/取消注释(N)          |    Ctrl+Shift+/     |
|  7   |          编辑/选择/转到行(G)           |       Ctrl+G        |
|  8   |       工具/源/PL/SQL Beautifier        |    Ctrl+Shift+F     |
|  9   |       删除行(Editor:Delete Line)       |       Ctrl+Y        |
|  10  | PLSQL切换窗口(Editor:Navigate Forward) | Alt+Left上一个窗口  |
|  11  |  PLSQL切换窗口(Editor:Navigate Back)   | Alt+Right下一个窗口 |


**********************************************************************************************************



<font size=6.5 color='red'>持续更新中。。。。。。</font>
