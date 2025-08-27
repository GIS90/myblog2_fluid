---
title: 初学Atom
index_img: /img_index/index/20180310-001.jpg
tags: [Atom, IDE, Python]
categories:
  - [工具集]
keywords: Atom, 编辑器, python, 开发, 代码 IDE, 插件
abbrlink: 26903
date: 2018-03-10 21:57:16
update: 2018-03-12 20:03:20
desc: 简单介绍一下平常使用的编译器：atom，支持各种语言以及插件，很实用的IDE
---
日常开发中习惯了Pycharm，但是平常记录点什么东西，最好要有一个自己的文本编译器，比如：sublime、vscode、notepad等等。

<!--more-->

<hr />

### 1、介绍

下面介绍一下我用的atom，安装以及其他的设置我不做介绍了，网上教程一大推，主要介绍一下我用的几个插件：
查看安装的包【**apm list**】
 ![apm list](chuxueatom_list.jpg)

### 2、常用快捷键

| 快捷键          | 功能                                                                          | 级别   |
| --------------- | ----------------------------------------------------------------------------- | ------ |
| shift+command+p | 功能查找命令行                                                                | 🌟🌟🌟 |
| shift+command+o | 打开目录                                                                      | 🌟🌟🌟 |
| command+,       | 配置                                                                          | 🌟🌟🌟 |
| shift+commadn+d | 复制当前行到下一行                                                            | 🌟🌟🌟 |
| shift+control+k | 删除光标所在当前行                                                            | 🌟🌟🌟 |
| command+\       | 显示或隐藏目录树                                                              | 🌟🌟   |
| ctrl-0          | 目录树与文本编辑区切换（切换到目录树：a新增【add】m修改【modify】delete删除） | 🌟🌟🌟 |
| alt+left        | 单词开始                                                                      | 🌟🌟   |
| alt+right       | 单词结束                                                                      | 🌟🌟   |
| command+left    | 行开始                                                                        | 🌟🌟🌟 |
| command+right   | 行结束                                                                        | 🌟🌟🌟 |
| command+up      | 文件开始                                                                      | 🌟🌟   |
| command+down    | 文件结束                                                                      | 🌟🌟   |
| command-b       | 打开文件之间的切换                                                            | 🌟🌟   |
| command-t       | 目录文件之间的切换                                                            | 🌟🌟   |
| command-r       | 查找与替换                                                                    | 🌟🌟🌟 |
| shift+command+r | 项目查找与替换                                                                | 🌟🌟🌟 |
| command+/       | 注释                                                                          | 🌟🌟🌟 |

### 3、基础插件

* #### 3.1、activate-power-mode

【功能】: 敲代码炫酷效果
【快捷键】: 无
【简述】: 怎么说呢，这个插件对于实际开发没有卵用，but可以装B，足以。
![activate-power-mode](chuxueatom_power.jpg)

* #### 3.2、atom-beautify

【功能】: 格式化当前文件
【快捷键】: control + option + b
【简述】: 可以格式化当前文件，前提是这个文件可以被识别，如果你的文件不带后缀直接去格式化，会warning，建议保存完文件在进行格式化。直接快捷键control + option + b格式化，插件会自动识别，记不得快捷键格式化可以先shift + command + p调出命令行查找，在入beautify editor，也可以直接输入beautify + 计算机语言【例如：python】，确认既可以。

* #### 3.3、autocomplete-paths

【功能】: 补全路径
【快捷键】: 无
【简述】: 在写代码时候，有时需要写文件路径，装上此插件，会自动完善文件路径，不过相信没有几个真正上线的project会写死文件路径吧。

* #### 3.4、autocomplete-python

【功能】: python补全
【快捷键】: 无
【简述】: 不多说，python开发者必备，前提是你在.py文件中进行。

* #### 3.5、autosave

【功能】: 自动保存
【快捷键】: 无
【简介】: 如果你是个懒人，跟我一样，最好安装这个插件。

* #### 3.6、file-icons

【功能】: 文件类型icon
【快捷键】: 无
【简介】: 左侧目录文件类型icon显示。

* #### 3.7、highlight-selected

【功能】: 高亮选中
【快捷键】: 无
【简介】: 高亮选中字符，同时所有相同字符高亮。

* #### 3.8、hyperclick

【功能】: 方法跳转
【快捷键】: command + click左键
【简介】: 用于方法跳转，用完之后，暂时我不知道怎么跳转回去，有知道的可以留言进行交流。

* #### 3.9、minimap && minimap-highlight-selected

【功能】: minimap显示
【快捷键】: 无
【简介】: 以一个minimap进行显示当前的文件内容，方便进行拖拽，加minimap-highlight-selected插件，方便你在选中一个之后在minimap中进行查看，位置可以配置。

* #### 3.10、platformio-ide-terminal

【功能】: terminal工具
【快捷键】: 新建：shift + command + T; 关闭：shift + command + X; 前一个：shift + command + J; 后一个：shift + command + K
【简介】: 方便控制台直接在atom中使用，省去切换。如果忘记快捷键，还是建议shift + command + p进行关键字terminal搜索，一目了然。

* #### 3.11、project-manager

【功能】: project存储
【快捷键】: add: shift + command + O;list：control + command + p
【简介】: 新open一个atom是不记录上次的文件的，引用插件可以把project进行存储，每次方便进行打开。还是那句话，如果忘记快捷键，还是议shift + command + p进行关键字project搜索，一目了然。

* #### 3.12、script

【功能】: 脚本执行
【快捷键】: 执行：command + i; 关闭问题view：control + w; 选中执行: shift command + i
【简介】: 如果因为atom不能直接执行你写的脚本发愁，那么这个插件就完美的解决你的问题。当然还有一个其他的解决办法就是shift + command + t调用terminal去执行。显示还是command + i方便，哈哈。

* #### 3.13、markdown

【功能】: markdown编辑
【快捷键】: shift + control + m 显示md效果
【简介】:
markdown-image-paste 图片处理
markdown-preview-plus 预览效果
markdown-scroll-sync 同步滚动
markdown-table-editor 表格处理

### 4、学习

快捷键大全：https://www.jianshu.com/p/e33f864981bb

### 5、建议

Atom可以集成git插件，以及flake8，但是我觉得还是这2个命令还是在item2上去敲，这2个命令必须得日常所用，敲得多熟悉的也就越深，玩python的人，当然是linux的基础命令都得熟悉。
