---
title: Conda命令初始化PowerShell
index_img: /img_index/index/20260821-001.png
tags:
  - Conda
  - Python
  - uv
  - Powershell
categories:
  - 工具集
abbrlink: 19481
date: 2026-08-21 21:58:22
updated: 2026-08-21 21:58:22
---

Windows电脑环境上安装了conda命令，用于处理多版本Python共存环境问题，但是每次使用conda命令都需要先激活环境，比较麻烦。所以，我想找到一种方法，每次打开powershell都自动激活conda环境。


<!--more-->
<hr />



### 初始化环境
在powershell中执行以下命令，初始化环境。
```powershell
& "D:\miniconda3\Scripts\conda.exe" shell.powershell hook | Out-String | Invoke-Expression
conda init powershell
```
### 验证环境
重新打开后，输入命令。

![效果图](1.png)
