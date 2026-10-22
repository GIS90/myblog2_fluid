---
title: "\U0001F6A2Pandoc 文件格式转换命令"
index_img: /img_index/index/20240122-001.png
tags:
  - 命令工具
  - pandoc
categories:
  - 工具集
abbrlink: 18040
date: 2024-01-22 22:55:12
updated: 2024-01-22 22:55:12
---



`Pandoc` 是一个强大的文档转换工具，支持多种格式之间的相互转换，如 Markdown、HTML、LaTeX、Word（.docx）、PDF、EPUB 等。本教程将介绍如何使用 Pandoc 的命令行进行基本的文档转换操作。

<!--more-->
<hr />

## 1、安装 Pandoc

### 1.1、Windows
前往 [**Pandoc 官网** ](https://pandoc.org/installing.html) 下载安装程序并安装。

### 1.2、macOS
使用 Homebrew 安装：
```bash
brew install pandoc
```

### 1.3、Linux (Ubuntu/Debian)
```bash
sudo apt-get install pandoc
```





## 2、基本语法结构

```bash
pandoc [选项] [输入文件] -o [输出文件]
```
- **输入文件**：可以是 Markdown、LaTeX、HTML 等格式。
- **输出文件**：可以指定输出格式（如 PDF、DOCX、HTML 等）。
- **选项**：控制转换行为（如模板、样式、元数据等）。
- **-o**: 指定输出文件名。



## 3、格式转换示例

### 3.1、Markdown 转 HTML
```bash
pandoc input.md -o output.html
```

### 3.2、Markdown 转 Word (.docx)
```bash
pandoc input.md -o output.docx
```

### 3.3、Markdown 转 PDF（需安装 LaTeX）
```bash
pandoc input.md -o output.pdf
```

> ⚠️ 注意：生成 PDF 需要系统中安装了 LaTeX 引擎（如 TeX Live 或 MiKTeX）。 

### 3.4、HTML 转 Markdown
```bash
pandoc input.html -o output.md
```
### 3.5、Word (.docx) 转 Markdown
```bash
pandoc input.docx -o output.md
```
### 3.6、Markdown → EPUB（电子书）
```bash
pandoc input.md -o output.epub
```



## 4、常用选项

| 选项                           | 说明                                 |
| :------------------------------: | :------------------------------- |
| `-f` 或 `--from`                 | 指定输入格式（例如：markdown, html, docx） |
| `-t` 或 `--to`                   | 指定输出格式                         |
| `-o` 或 `--output`               | 指定输出文件路径                     |
| `--standalone` 或 `-s`           | 生成完整的独立文档（如带完整 HTML 结构）|
| `--template=FILE`              | 使用自定义模板                         |
| `--toc` 或 `--table-of-contents` | 自动生成目录                      |
| `--number-sections`            | 自动编号章节                       |
| `--highlight-style`            | 设置代码高亮样式                    |

具体命令使用详情：`pandoc --help`。



## 5、其他示例

### 5.1、生成带目录的 HTML 文档
```bash
pandoc --standalone --toc input.md -o output.html
```

### 5.2、转换时应用 CSS 样式
```bash
pandoc input.md --css=style.css -o output.html
```

### 5.3、打印转换结果到终端

如果不指定 `-o` 输出文件，Pandoc 会将结果输出到终端：
```bash
pandoc input.md
```

### 5.4、多文件合并转换

可以一次转换多个文件并合并为一个输出文件：
```bash
pandoc file1.md file2.md -o combined.docx
```

### 5.5、使用模板生成个性化文档

Pandoc 支持使用模板来控制输出格式。例如使用默认模板生成一份 Word 文档：
```bash
pandoc input.md -o output.docx --template = my-template.docx
```
你可以通过以下命令查看默认模板内容：
```bash
pandoc -D default.docx > my-template.docx
```

### 5.6、使用元数据设置标题、作者和日期

创建一个 YAML 元数据头部的 Markdown 文件（例如 `metadata.md`）：
```yaml
\---
title: 我的报告
author: 张三
date: 2025-04-05
\---

\# 正文开始
```
然后转换为 PDF：
```bash
pandoc metadata.md -o report.pdf
```

### 5.7、插入 LaTeX 数学公式（适用于 PDF 输出）

在 Markdown 中插入数学公式：
```markdown
这是一个行内公式：$E = mc^2$

这是一个块级公式：
$$
\int_{a}^{b} f(x) dx
$$
```
转换为 PDF：
```bash
pandoc math.md -o math.pdf
```


## 6、进阶技巧

### 6.1、使用过滤器（Filter）

Pandoc 支持使用 Lua 或 JSON 过滤器对文档结构进行处理。
例如使用 `pandoc-citeproc` 来支持引用文献：
```bash
pandoc input.md --filter = pandoc-citeproc -o output.html
```

需要提前安装 `pandoc-citeproc`：
```bash
cabal install pandoc-citeproc
```
或者使用 pip（视环境而定）：
```bash
pip install pandoc-citeproc
```


## 7、📚参考资料

- [Pandoc 官方手册](https://pandoc.org/MANUAL.html)
- [Pandoc GitHub 仓库](https://github.com/jgm/pandoc)
- [Pandoc 用户论坛](https://groups.google.com/g/pandoc-discuss)