---
title: 🔧SVN 命令教程
index_img: /img_index/index/20250905-001.jpg
tags:
  - 命令工具
  - SVN
categories:
  - 工具集
abbrlink: 12345
date: 2025-09-05 23:30:00
updated: 2025-09-05 23:30:00
---

`SVN` (Subversion) 是一个流行的集中式版本控制系统，广泛应用于软件开发、文档管理等领域。除了SVN就是git了，之前的文章介绍过git命令了，本教程将介绍SVN的基本概念和常用命令，帮助您快速掌握SVN的使用方法。

<!--more-->
<hr />

## 1、安装 SVN

### 1.1、Windows
前往**SVN**官网或 **TortoiseSVN**官网下载安装程序并安装，下载地址：
- SVN：https://sliksvn.com/download/
- TortoiseSVN：https://tortoisesvn.net/downloads.html


### 1.2、macOS
使用 Homebrew 安装：
```bash
brew install svn
```

### 1.3、Linux (Ubuntu/Debian)
```bash
sudo apt-get install subversion
```

安装完成后，可以使用以下命令检查SVN版本：
```bash
svn --version
```



## 2、SVN 基本概念

在开始使用SVN命令之前，先了解几个基本概念：

- **工作副本（Working Copy）**：用户本地编辑的代码目录
- **仓库（Repository）**：存储所有版本历史的中央服务器
- **提交（Commit）**：将本地修改保存到仓库
- **更新（Update）**：从仓库获取最新版本
- **冲突（Conflict）**：多人同时修改同一文件导致的冲突
- **分支（Branch）**：独立的开发线
- **合并（Merge）**：将一个分支的更改应用到另一个分支



## 3、基本命令操作

### 3.1、检出仓库（Checkout）
从服务器检出一个工作副本到本地：

```bash
svn checkout [URL] [本地目录]
```

示例：
```bash
svn checkout https://svn.example.com/repos/myproject ./myproject
```

### 3.2、更新工作副本（Update）
更新本地工作副本到最新版本：

```bash
svn update
```

更新到指定版本：
```bash
svn update -r [版本号]
```

### 3.3、查看状态（Status）
查看工作副本中文件的状态：

```bash
svn status
```

输出中的状态码含义：
- `A`：已添加
- `C`：存在冲突
- `D`：已删除
- `M`：已修改
- `?`：不在版本控制中

### 3.4、添加文件（Add）
将新文件添加到版本控制：

```bash
svn add [文件/目录]
```

添加所有未跟踪文件：
```bash
svn add --force .
```

### 3.5、提交更改（Commit）
将本地修改提交到服务器：

```bash
svn commit -m "提交信息"
```

### 3.6、查看日志（Log）
查看提交历史：

```bash
svn log
```

查看特定文件的历史：
```bash
svn log [文件路径]
```

### 3.7、比较差异（Diff）
比较工作副本与版本库的差异：

```bash
svn diff
```

比较两个版本之间的差异：
```bash
svn diff -r [版本1]:[版本2]
```

### 3.8、撤销修改（Revert）
撤销工作副本中的修改：

```bash
svn revert [文件/目录]
```

撤销所有修改：
```bash
svn revert --recursive .
```



## 4、文件管理命令

### 4.1、复制文件（Copy）
在版本控制下复制文件：

```bash
svn copy [源文件] [目标文件]
```

### 4.2、移动/重命名文件（Move）
移动或重命名文件：

```bash
svn move [源文件] [目标文件]
```

### 4.3、删除文件（Delete）
从版本控制中删除文件：

```bash
svn delete [文件/目录]
```

### 4.4、查看文件内容（Cat）
查看指定版本的文件内容：

```bash
svn cat -r [版本号] [文件路径]
```

### 4.5、查看文件历史（Blame）
查看文件每行的修改记录：

```bash
svn blame [文件路径]
```



## 5、分支与合并

### 5.1、创建分支（Branch）
创建一个新分支：

```bash
svn copy [URL/trunk] [URL/branches/新分支名] -m "创建分支说明"
```

### 5.2、切换分支（Switch）
切换到其他分支：

```bash
svn switch [URL/branches/分支名]
```

### 5.3、合并分支（Merge）
将分支合并到主干：

1. 首先切换到主干目录：
```bash
svn switch [URL/trunk]
```

2. 然后执行合并操作：
```bash
svn merge [URL/branches/分支名]
```

3. 解决冲突后提交：
```bash
svn commit -m "合并分支到主干"
```

### 5.4、标记版本（Tag）
为特定版本创建标签：

```bash
svn copy [URL/trunk] [URL/tags/标签名] -m "创建标签说明"
```



## 6、冲突解决

当多人同时修改同一文件时，可能会发生冲突。以下是解决冲突的步骤：

1. 首先查看冲突文件：
```bash
svn status
```

2. 冲突的文件会显示为 `C` 状态，打开文件查看冲突标记：
```
<<<<<<< .mine
你的修改内容
=======
其他人的修改内容
>>>>>>> .rXXX
```

3. 编辑文件，手动解决冲突

4. 标记冲突已解决：
```bash
svn resolved [冲突文件]
```

5. 最后提交解决冲突后的文件：
```bash
svn commit -m "解决冲突"
```



## 7、SVN 配置

### 7.1、设置全局用户信息
配置SVN全局用户名和邮箱：

```bash
svn config --global user.name "你的名字"
svn config --global user.email "你的邮箱"
```

### 7.2、忽略文件
创建或编辑 `.svnignore` 文件，添加不需要版本控制的文件或目录：

```
*.log
*.tmp
node_modules/
build/
```

然后将 `.svnignore` 文件添加到版本控制：
```bash
svn propset svn:ignore -F .svnignore .
svn commit -m "添加忽略规则"
```



## 8、高级命令

### 8.1、查看版本库信息（Info）
查看工作副本或URL的信息：

```bash
svn info
```

### 8.2、清理工作副本（Cleanup）
当SVN操作中断时，使用此命令清理工作副本：

```bash
svn cleanup
```

### 8.3、导出文件（Export）
导出一个不包含 `.svn` 目录的干净副本：

```bash
svn export [URL] [本地目录]
```

### 8.4、创建补丁（Diff）
生成差异补丁文件：

```bash
svn diff > changes.patch
```

### 8.5、应用补丁（Patch）
使用 svn-apply 或系统 patch 命令应用补丁：
```bash
patch -p0 < changes.patch
```



## 9、SVN 钩子脚本

SVN钩子脚本是在特定事件发生时自动执行的脚本。常见的钩子脚本包括：

- **pre-commit**：提交前执行，可用于代码检查
- **post-commit**：提交后执行，可用于自动部署
- **pre-revprop-change**：修改版本属性前执行

钩子脚本通常放置在版本库的 `hooks` 目录下。



## 10、📚参考资料

- SVN 官方文档：https://subversion.apache.org/docs/
- SVN 中文教程：https://www.runoob.com/svn/svn-tutorial.html

