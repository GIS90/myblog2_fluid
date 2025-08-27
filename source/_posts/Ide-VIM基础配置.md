---
title: VIM基础配置
index_img: /img_index/index/20201221-001.jpeg
tags: [VIM, IDE, Linux]
categories:
  - [工具集]
top: false
abbrlink: 36465
date: 2020-12-21 11:18:09
updated: 2020-12-21 11:18:09
desc: VIM基础配置
keywords: VIM, 编辑器, Linux, 配置
---



总结分享一下**VIM基础配置**。


<!--more-->
<hr />

### 1、配置文件
```
" A vimrc file.
"
" User:	mingliang.gao
" Create time: 2020-12-20
"

" ================================ base setting ================================
set nocompatible
set number
filetype plugin indent on
set history=1000
set background=dark
syntax on
syntax enable
set ruler
set showmatch
set laststatus=2
set showmode
set ignorecase
set nofoldenable " 不折叠代码
set showcmd
set mouse=a    " 开启鼠标
set wrap
set noerrorbells
set backspace=2

" 设置molokai主题
let mapleader=','
let g:molokai_original = 1
let g:rehash256 = 1
set t_Co=256
colorscheme molokai

" search
set hlsearch
set incsearch

" 文件编码
set termencoding=utf-8
set encoding=utf8
set fileencodings=utf8,ucs-bom,gbk,cp936,gb2312,gb18030

" 分割布局
set splitbelow
set splitright

" 用浅颜色高亮当前行，列
set cul
set cuc
highlight CursorLine   cterm=NONE ctermbg=black ctermfg=blue guibg=NONE guifg=NONE
highlight CursorColumn cterm=NONE ctermbg=black ctermfg=blue guibg=NONE guifg=NONE

" 缩进方式
set autoindent
set smartindent
set tabstop=4
set shiftwidth=4

" 备份
if has("vms")
  set nobackup		" do not keep a backup file, use versions instead
else
  set backup		" keep a backup file (restore to previous version)
  if has('persistent_undo')
    set undofile	" keep an undo file (undo changes after closing)
  endif
endif

" swap文件与undo文件
set swapfile
set undofile

"=====================================================================================

```

### 2、配置说明

#### 1、基础配置说明

- set nocompatible：不兼容vi
- set number：行号
- filetype plugin indent on：自动识别文件类型
- set history=1000：回退的历史记录
- set background=dark：主题
- syntax on：语法高亮开启
- set ruler：设置XX行XX列
- set showmatch：括号匹配
- set laststatus=2：一直显示vim底部状态信息
- set showmode：显示当前vim模式（正常、编辑、视图）
- set ignorecase：忽略大小写搜索
- set nofoldenable " 不折叠代码
- set showcmd：如果!+cmd执行命令，显示执行的命令
- set mouse=a：开启鼠标
- set wrap：自动换行
- set noerrorbells：关闭错误提示声音
- set backspace=2：可以在编辑模式下使用backspace

#### 2、设置molokai主题

github地址：https://github.com/tomasr/molokai
文件放在/Users/gaomingliang/.vim/colors目录下。
- set t_Co=256：设置配置
- colorscheme molokai：设置主题

#### 3、search搜索配置

- set hlsearch：设置查询的高亮显示
- set incsearch：增强vim中自带的?（向下搜索）和／（向上搜索）搜索功能

#### 4、文件编码

- set termencoding=utf-8：输出到客户终端（Term）采用的编码类型
- set encoding=utf8：于缓冲的文本、寄存器、脚本文件等
- set fileencodings=utf8,ucs-bom,gbk,cp936,gb2312,gb18030：设置写入文件支持的编码

#### 5、分割布局

- set splitbelow：横向-下
- set splitright：纵向-右
很少用到，习惯多个tab，用taglist插件进行切换。

#### 6、用浅颜色高亮当前行，列
- set cul
- set cuc
- highlight CursorLine   cterm=NONE ctermbg=black ctermfg=blue guibg=NONE guifg=NONE
- highlight CursorColumn cterm=NONE ctermbg=black ctermfg=blue guibg=NONE guifg=NONE

#### 7、缩进方式

- set autoindent：自动化缩进，VIM会根据打开文件类型进行缩进。
- set smartindent：同上。
- set tabstop=4：tab键为4个空格
- set shiftwidth=4：程序自动缩进的空格数，默认缩进

#### 8、备份

```
if has("vms")
 set nobackup		" do not keep a backup file, use versions instead
else
 set backup		" keep a backup file (restore to previous version)
 if has('persistent_undo')
   set undofile	" keep an undo file (undo changes after closing)
 endif
endif
```
使用这个命令会打开一个文件进行备份一个文件，不建议使用，文件太多了。

#### 9、swap文件与undo文件

- set swapfile：自动多一个.swap文件。
- set undofile：自动多一个.undo文件，用来回退的文件。


### 3、结束语
<font color="red" size="5">持续更新中～</font>
