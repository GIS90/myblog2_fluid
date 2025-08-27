---
title: Select2的基础使用
index_img: /img_index/index/20201021-001.jpg
categories:
  - [前端开发]
tags: [HTML, 前端组件]
abbrlink: 47159
date: 2020-10-21 19:58:08
updated: 2020-10-21 19:58:08
desc: 前端组件select2的基础使用
keywords: HTML, select, select2, option, 前端组件
---


{% note success %}
写html的人必定用过select标签，样式确实有些。。。如果自己写样式可以。
上网找select组件，发现select2，有好的组件咱们直接用就好了。
{% endnote %}


<!--more-->

<hr />

#### 1、下载地址
下载之后放在本地。
CSS：https://cdn.jsdelivr.net/npm/select2@4.1.0-beta.1/dist/css/select2.min.css
JS：https://cdn.jsdelivr.net/npm/select2@4.1.0-beta.1/dist/js/select2.min.js

#### 2、Demo
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link href="select2-4.1.0/select2.min.css" rel="stylesheet" />
    <script src="select2-4.1.0/select2.min.js"></script>
</head>
<body>
    <select class="js-example-basic-single" name="state" multiple="multiple" style="width: 100%;">
      <option value="AL">Alabama</option>
      <option value="WY">Wyoming</option>
    </select>
</body>
<script>
    $(document).ready(function() {
        $('.js-example-basic-single').select2();
    });
</script>
</html>
```
需要其他写法的可以查看官网。

#### 3、配置
```
$('#problem_types').select2({
    placeholder: '可多选，最多支持4个一级分类，支持模糊查询',
    maximumSelectionLength: 4,
    allowClear: true,
    closeOnSelect: false,
    selectionCssClass: 'info_red',
    //dropdownCssClass: 'info_red',
    tags: false,
    tokenSeparators: [',', ' '], // 多选 - 输入逗号和空格会自动完成一个选项 前提是：tags: true
    //theme: "classic"
});
```
- multiple：支持多选
- placeholder：标签的默认文字显示
- maximumSelectionLength：最大选择的个数
- allowClear：启用一键清除选择的功能
- closeOnSelect：是否选择一个就关闭option选择栏
- selectionCssClass：选择的options类x的样式类
- dropdownCssClass：下拉列表的文字样式
- tags：是否启动自定义选择的功能
- tokenSeparators：输入逗号和空格会自动完成一个选项 前提是：tags: true
- theme：select2的样式，目前只看到了classic还有默认

#### 4、学习参考

官网：https://select2.org/
配置：https://select2.org/configuration/options-api
