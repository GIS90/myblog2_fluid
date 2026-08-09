---
title: Windows的PowerShell增强版
index_img: /img_index/index/20260809-001.jpeg
tags: [Windows, 命令工具]
categories: [工具集]
abbrlink: 35006
date: 2026-08-09 09:07:35
updated: 2026-08-09 09:07:35
---

用惯了MacOS的控制台zsh，转用Windows上cmd，总会有点不习惯，上网搜了一下发现在Windows上也可以打造PowerShell + oh-my-posh，实现相同的效果，并且支持Linux命令（只是内嵌了别名，不是真正支持，那也够用了）。


<!--more-->
<hr />

## 效果

![](1.jpg)

## 操作流程
oh-my-posh配置【一定要用power shell】
### 1、配置环境变量
配置电脑高级环境变量配置
```
【POSH_THEMES_PATH】C:\Program Files\WindowsApps\ohmyposh.cli_29.31.1.0_x64__96v55e8n804z4\themes
```
### 2、安装插件
安装所用的插件，基于PowerShell进行命令按照。
```
winget install Git.Git
Install-Module PSReadLine -Scope CurrentUser -Force
Install-Module Terminal-Icons -Scope CurrentUser -Force
```
### 3、配置文件
使用命令notepad/echo $PROFILE，打开配置文件，具体配置见下面。
### 4、配置Terminal外观
根据个人的喜爱来配置PowerShell的外观，使用图片背景+不透明度55%，很酷炫。

## 配置文件
直接上文件，粘贴即可。
```
# 获取所有主题文件列表->随机选一个->加载选中的主题
$themes = Get-ChildItem $env:POSH_THEMES_PATH -Filter "*.omp.json"
$randomTheme = $themes | Get-Random
Write-Host "=======>Current loading theme: [$($randomTheme.BaseName)]<=======" -ForegroundColor Cyan
oh-my-posh init pwsh --config $randomTheme.FullName | Invoke-Expression


# 安装模块【命令安装】
# winget install Git.Git
# Install-Module PSReadLine -Scope CurrentUser -Force
# Install-Module Terminal-Icons -Scope CurrentUser -Force

# 导入模块
Import-Module posh-git          
Import-Module PSReadLine       
Import-Module Terminal-Icons


# PSReadLine配置 ---------------------------------------------------------------------------------------------------------
# 设置预测文本来源为历史记录 
Set-PSReadLineOption -PredictionSource History 
# 设置 Tab 为菜单补全和 Intellisense 
Set-PSReadLineKeyHandler -Key "Tab" -Function MenuComplete 
# 每次回溯输入历史，光标定位于输入内容末尾 
Set-PSReadLineOption -HistorySearchCursorMovesToEnd 
# 设置向上键为后向搜索历史记录 
Set-PSReadLineKeyHandler -Key UpArrow -Function HistorySearchBackward 
# 设置向下键为前向搜索历史纪录 
Set-PSReadLineKeyHandler -Key DownArrow -Function HistorySearchForward
# -----------------------------------------------------------------------------------------------------------------------------
```

## 常见快捷键

| 快捷键                          | 操作                             |
| :----------------------------: | :----------------------------------- |
| Ctrl + ,	打开设置	| 打开 Windows Terminal 设置界面 |
| Ctrl + Shift + T	| 新建标签页，在当前窗口中新增一个标签页 |
| Ctrl + Shift + N	| 新建窗口，打开一个全新的独立窗口 |
| Ctrl + Shift + W	| 关闭标签页 |
| Shift + Alt + -（减号） | 水平拆分窗格 |
| Shift + Alt + +（加号） | 垂直拆分窗格 |
| Shift + Alt + 上下左右 | 调整分屏窗格大小 |
| Alt + 上下左右 | 切换分屏光标位置 |
| Ctrl + Tab | 切换窗口 |
| Ctrl + -（减号） | 减小字体 |
| Ctrl + +（加号） | 增大字体 |
| Ctrl + Shift + P	| 命令面板 |