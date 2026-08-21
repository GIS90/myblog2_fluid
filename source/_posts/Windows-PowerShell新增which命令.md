---
title: Windows的PowerShell新增which命令
index_img: /img_index/index/20260728-001.png
tags:
  - PowerShell
categories:
  - Windows
abbrlink: 10392
date: 2026-07-28 22:40:27
updated: 2026-07-28 22:40:27
---


发现在PowerShell找命令的路径时，发现没有which命令，只能使用Get-Command或where.exe来查找命令的路径，喜欢Linux命令的简捷性，想在PowerShell中也能直接使用which命令。



<!--more-->
<hr />

#### 打开PowerShell配置文件
```powershell
# 编辑配置文件
notepad $PROFILE
```

#### 在打开的记事本中添加以下内容并保存：
```powershell
Set-Alias which Get-Command
```
重新打开 PowerShell 后，which 命令就可以直接使用了。


#### 测试查询Python路径
```powershell
which python
```
#### 结果
```text
CommandType     Name        Version    Source
-----------     ----        -------    ------
Application     python.exe  3.11.0     D:\miniconda3\python.exe
```

#### 拓展查询其他命令路径
用于查询目录下的所有文件和子目录
```powershell
Set-Alias ll Get-ChildItem -Force
```



