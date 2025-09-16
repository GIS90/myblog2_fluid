---
title: Windows的winget命令教程
index_img: /img_index/index/20250913-001.png
categories:
  - [Windows]
tags: [winget]
desc: windows的winget命令教程
keywords: Windows, winget
abbrlink: 63286
date: 2025-09-13 20:28:22
updated: 2025-09-13 20:28:22
---


### 1、什么是 Winget？

Winget 是 Windows 包管理器（Windows Package Manager）的命令行工具，由微软开发。它允许用户通过命令行轻松查找、安装、升级、配置和卸载 Windows 上的应用程序。

<!-- more -->

### 2、系统要求

Windows 10及以上版本

### 3、安装 Winget

#### 3.1、通过 Microsoft Store 安装（推荐）
1. 打开 Microsoft Store
2. 搜索 "App Installer"
3. 点击"获取"进行安装

#### 3.2、手动安装
从 GitHub 发布页面下载最新版本的 `.msixbundle` 文件并安装：
https://github.com/microsoft/winget-cli/releases

### 4、基础命令

#### 4.1、搜索软件包
```bash
# 基本搜索
winget search <软件名称>

# 示例：搜索 Chrome 浏览器
winget search chrome

# 使用通配符搜索
winget search *python*

# 按发布者搜索
winget search --publisher Google
```

#### 4.2、安装软件
```bash
# 基本安装
winget install <软件包ID或名称>

# 示例：安装 Google Chrome
winget install Google.Chrome

# 安装特定版本
winget install Google.Chrome --version 91.0.4472.124

# 安装到特定位置
winget install Google.Chrome --location "D:\Applications"

# 静默安装（无交互）
winget install Google.Chrome --silent

# 接受所有提示（自动同意许可协议等）
winget install Google.Chrome --accept-package-agreements --accept-source-agreements
```

### 5、升级软件
```bash
# 查看可升级的软件
winget upgrade

# 升级所有软件
winget upgrade --all

# 升级特定软件
winget upgrade Google.Chrome

# 升级时跳过特定软件
winget upgrade --all --exclude Google.Chrome
```

### 6、卸载软件
```bash
# 卸载软件
winget uninstall <软件包ID或名称>

# 示例：卸载 Google Chrome
winget uninstall Google.Chrome

# 使用精确ID卸载
winget uninstall --id Google.Chrome
```
### 7、显示软件信息
```bash
# 查看软件详情
winget show <软件包ID或名称>

# 示例：查看 Firefox 详情
winget show Mozilla.Firefox
```

### 8、高级用法
#### 8.1、导出和导入配置
```bash
# 导出已安装软件列表
winget export -o packages.json

# 从文件安装软件
winget import -i packages.json
```

#### 8.2、列出已安装软件
```bash
# 列出所有已安装软件
winget list

# 按名称过滤
winget list --query "Python"

# 按来源过滤
winget list --source winget
```
#### 8.3、源管理
```bash
# 列出所有源
winget source list

# 添加新源
winget source add <源名称> <源URL>

# 重置为默认源
winget source reset
```
#### 8.4、验证软件哈希值
```bash
winget validate <安装包路径>
```
### 9、实用技巧和提示
#### 9.1、使用交互模式
```bash
# 交互式安装
winget install --interactive
```
#### 9.2、忽略依赖项
```bash
winget install <软件包> --ignore-dependencies
```
#### 9.3、指定架构
```bash
winget install <软件包> --architecture x64
# 或
winget install <软件包> --architecture x86
```

#### 9.4、批量安装开发工具
```bash
# 创建安装脚本
winget install Microsoft.VisualStudioCode
winget install Git.Git
winget install Python.Python.3.9
winget install Docker.DockerDesktop
```

#### 9.5、查看帮助信息
```bash
# 查看全局帮助
winget --help

# 查看具体命令帮助
winget install --help
```
#### 9.6、保存安装日志
```bash
winget install <软件包> --log <日志文件路径>
```

### 10、常见问题解决
#### 10.1、权限不足
使用管理员权限打开 PowerShell 或命令提示符

#### 10.2、源更新失败
```bash
winget source update
```
#### 10.3、软件包冲突
使用精确的包ID而不是名称

#### 10.4、版本不兼容
使用 --version 参数指定兼容版本


### 11、资源推荐
- 官方文档：https://learn.microsoft.com/zh-cn/windows/package-manager/
- 软件包仓库：https://github.com/microsoft/winget-pkgs
- 社区讨论：https://github.com/microsoft/winget-cli/discussions

