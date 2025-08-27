---
title: CkEditor的安装与基础使用
index_img: /img_index/index/20210118-001.jpg
categories:
  - [前端开发]
tags: [HTML, 前端组件, CkEditor, 富文本编辑器]
top: false
abbrlink: 9743
date: 2021-01-18 21:43:12
updated: 2021-01-18 21:43:12
desc: 富文本编辑器CkEditor的教程
keywords: CkEditor, 富文本, 编辑器, HTML
---
{% label danger@CKEditor系列教程（一） %}

<!--more-->

<hr />

CkEditor有4版本以及最新5系列版本，看了内容，感觉4就够了，有兴趣的人可以下载5。

#### 下载地址

https://ckeditor.com/ckeditor-4/download/

CkEditor4有4个版本下载：base（基础版）、standard（标准版）、full（完整版）、custome（定制版）。
![](version.png)
说白了就是插件多少的问题，建议选择full版本，反正项目部署到服务器，CkEditor插件每次也不会更新，不会影响任何问题。

其他方式安装：https://ckeditor.com/docs/ckeditor4/latest/guide/dev_installation.html

#### Demo

> HTML

```
<!DOCTYPE HTML>
<html lang="en-US">
<head>
    <meta charset="UTF-8">
    <title>ditor demo</title>
    <script type="text/javascript" src="ckeditor_4.15.1_full_easyimage/ckeditor.js"></script>
</head>
<body>
    <button onclick="show()">展示</button>
    <br>
    <br>
    <form>
        <textarea name="articleArea" id="articleArea" rows="10" cols="80">
        </textarea>
    </form>

</body>
<script>
    // with a CKEditor 4, instance, using default configuration.
    CKEDITOR.replace( 'articleArea' );

    function show() {
         var editor_data = CKEDITOR.instances.articleArea.getData();
        console.log(editor_data)
    }

</script>
</html>
```

show方法是打印内容。

#### 学习参考

官网：https://ckeditor.com/
CKEditor系列教程（一）：`<a href="/articles/9743/" target="_blank" class="block_project_a">`CkEditor的安装与基础使用`</a>`
CKEditor系列教程（二）：`<a href="/articles/32150/" target="_blank" class="block_project_a">`CkEditor的配置说明`</a>`
CKEditor系列教程（三）：`<a href="/articles/32287/" target="_blank" class="block_project_a">`CkEditor图片上传到服务端`</a>`
