---
title: DB2自动化全量备份
index_img: /img_index/index/20210527-001.jpg
categories:
  - [数据库]
tags: [DB2]
top: false
abbrlink: 16336
date: 2021-05-27 22:35:12
updated: 2021-05-27 22:35:12
desc: Win服务器上DB2全量自动化备份bat脚本
keywords: DB2, db2, 数据库, 备份, backup
---


{% note primary %}
项目上使用的是DB2数据库，全库备份一直是手动备份，自动化的东西没有代码解决不了的问题，初识bat自动化脚本，写的也很简单，分享一下。
{% endnote %}

{% label warning@DB2 %} {% label default@backup %} {% label info@自动化 %}

<!--more-->
<hr />

> 实现流程

- 设置执行命令的编码，这个与运行服务器db2cmd的编码保持一致，设置936，如果是windows8、10等设置65001
- 备份信息的配置，包含数据库名称、用户、密码、存储目录、日志文件
- 存储目录不存在新建、日志文件不存在新建
- db2全库备份
- 记录执行结果

目前实现了基础的备份功能，后续加上彩色打印、自动化删除指定日期前备份文件功能，<font size=6.5 color='red'>持续更新中。。。。。。</font>

```
@echo off
:: CMD默认为65001编码，使用db2cmd执行需要设置936
chcp 936
SETLOCAL EnableDelayedExpansion
for /F "tokens=1,2 delims=#" %%a in ('"prompt #$H#$E# & echo on & for %%b in (1) do rem"') do (
  set "DEL=%%a"
)

title DB2 database backup
cls

REM +-------------------------------------------------------------+
REM + Base info:                                                  +
REM + -- auhtor: mingliang.gao                                    +
REM + -- time: 2021/05/31                                         +
REM + -- version: v1.0.1                                          +
REM + -- system: Windows                                          +
REM + -- desc: DB2 Windows all backup script                      +
REM +                                                             +
REM + Execute method:                                             +
REM + -- auto: db2cmd + task                                      +
REM + -- manual: all path run the script with db2cmd              +
REM +                                                             +
REM + Debug:                                                      +
REM + -- 自定义位置加入pause                                      +
REM +                                                             +
REM + 打印乱问题:                                                 +
REM + -- 1.设置编码与db2cmd编码一致，根据服务器db2cmd编码设定,    +
REM +      db2cmd右键->属性->选项中查看编码                       +
REM + -- 2.文件用文本编辑器打开，另存为ANSI编码格式               +
REM +                                                             +
REM + Useage:                                                     +
REM + -- 1.修改chcp编码，以运行服务器db2cmd编码为准               +
REM + -- 2.修改backup config                                      +
REM + -- 3.选择手动或者自动运行方式                               +
REM +                                                             +
REM + Others:                                                     +
REM + -- 1.设置定时任务时，程序选择db2cmd命令，参数选择备份脚本   +
REM +-------------------------------------------------------------+

REM +-------------------------------------------------------------+
REM + Backup info:                                                +
REM + -- ip: 16.19.209.67                                         +
REM + -- task: 每周六晚上11点备份                                 +
REM +-------------------------------------------------------------+

REM +-------------------------------------------------------------+
REM backup config
set db=mzlpas
set user=pas
set password=pas
set backup_dir=E:\BACKUP_MZLPAS
set backup_log=%backup_dir%\backup.log
REM +-------------------------------------------------------------+

for /f "tokens=1-4 delims=/-\ " %%a in ('date /t') do (
  set week=%%a
  set year=%%b
  set month=%%c
  set day=%%d
)
set curdate=%year%%month%%day%
echo ========================start backup========================
:: print base informations
echo 当前时间: %date% %time%
call :ColorText 0a "blue"
echo 数据库: %db%
echo 用户: %user%
echo 密码: %password%
echo 备份目录: %backup_dir%
echo 备份日志: %backup_log%

:: if not exist dir or log file, created
if not exist %backup_dir% (
  mkdir %backup_dir%
)
if not exist %backup_log% (
  echo %db% backup log >> %backup_log%
)

:: backup command
db2 connect to %db% user %user% using %password%
db2 force applications all
db2 backup db %db% to %backup_dir% PARALLELISM 2 COMPRESS

echo.

:: record the backup result
if errorlevel 0 (
  echo %curdate%: success
  echo %curdate%: success >> %backup_log%
) else (
  echo %curdate%: failed
  echo %curdate%: failed >> %backup_log%
  exit
)

echo =========================end backup=========================
pause
exit
```
