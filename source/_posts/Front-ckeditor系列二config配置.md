---
title: CkEditor的配置说明
index_img: /img_index/index/20210123-001.jpeg
categories:
    - [前端开发]
tags: [HTML, 前端组件, CkEditor, 富文本编辑器]
abbrlink: 32150
date: 2021-01-23 22:35:15
updated: 2021-01-23 22:35:15
desc: 富文本编辑器CkEditor的教程
keywords: CkEditor, 富文本, 编辑器, HTML
---


{% label success@CKEditor系列教程（二） %} 

<!--more-->

<hr />

#### 配置
配置文件在插件的config.js，也可以自定义config js文件。
```
/**
 * @license Copyright (c) 2003-2020, CKSource - Frederico Knabben. All rights reserved.
 * For licensing, see https://ckeditor.com/legal/ckeditor-oss-license
 */

CKEDITOR.editorConfig = function( config ) {
    // 语言
    config.language = 'zh-cn';
    // placeholder设置
    config.editorplaceholder = '开始新的篇章......';
    // 设置宽高
    // config.width = 400;
    config.height = 500;
    // UI面板配色
    config.uiColor = '#F7B42C';
    // 编辑器样式：bootstrapck icy_orange kama minimalist moono moono-dark moonocolor office2013
    // default：moono-lisa
    config.skin = 'icy_orange';
    // 添加字体
    config.font_names='宋体/SimSun;新宋体/NSimSun;仿宋_GB2312/FangSong_GB2312;楷体_GB2312/KaiTi_GB2312;黑体/SimHei;微软雅黑/Microsoft YaHei;'+ config.font_names;

    // 添加该行代码关闭easyimage、cloudservices插件即可
    config.removePlugins = 'easyimage,cloudservices';

    // 工具栏是否可以被收缩
    config.toolbarCanCollapse = true;
    // 工具栏的位置，可选：bottom
    config.toolbarLocation = 'top';
    // 工具栏默认是否展开
    config.toolbarStartupExpanded = true;
    // 取消 “拖拽以改变尺寸”功能
    config.resize_enabled = false;
    // 当提交包含有此编辑器的表单时，是否自动更新元素内的资料
    config.autoUpdateElement = true;
    // 编辑器的z-index值
    config.baseFloatZIndex = 10000;

    // 工具栏
    config.toolbar = [
        { name: 'document', groups: [ 'mode', 'document', 'doctools', 'undo'], items: [ 'NewPage', 'Save', 'Preview', 'Print', '-', 'Templates' , '-', 'Undo', 'Redo' ] },
        { name: 'editing', groups: [ 'find', 'selection', 'spellchecker' ], items: [ 'Find', 'Replace', '-', 'SelectAll', '-', 'Scayt' ] },
        { name: 'clipboard', groups: [ 'clipboard'], items: [ 'Cut', 'Copy', 'Paste', 'PasteText', 'PasteFromWord'] },
        //{ name: 'forms', items: [ 'Form', 'Checkbox', 'Radio', 'TextField', 'Textarea', 'Select', 'Button', 'ImageButton', 'HiddenField' ] },
        { name: 'insert', groups: [ 'links', 'clipboard'],  items: [ 'Link', 'Unlink', 'Anchor', '-', 'Image',  'Table', '-', 'HorizontalRule', 'Smiley', 'SpecialChar'] },
        '/',
        { name: 'basicstyles', groups: [ 'basicstyles', 'cleanup' ], items: [ 'Bold', 'Italic', 'Underline', 'Strike', '-', 'TextColor', 'BGColor', '-', 'Subscript', 'Superscript', '-', 'RemoveFormat' ] },
        { name: 'paragraph', groups: [ 'list', 'indent', 'blocks', 'align', 'bidi' ], items: [ 'NumberedList', 'BulletedList', '-', 'Outdent', 'Indent',  '-', 'JustifyLeft', 'JustifyCenter', 'JustifyRight', 'JustifyBlock', '-', 'BidiLtr', 'BidiRtl', 'Language' ] },
        '/',
        { name: 'styles', items: [ 'Styles', 'Format', 'Font', 'FontSize' ] },
        { name: 'others', items: [ 'Maximize', '-', 'ShowBlocks', 'Blockquote', 'CreateDiv', '-', 'Source'] }
    ];

    //上传图片API
    config.filebrowserImageUploadUrl = "/apis/uploadimg";
    // 去掉图片预览框的文字
    config.image_previewText = '';
    // 隐藏“超链接”与“高级选项”只留上传和预览按钮
    config.removeDialogTabs = 'image:advanced;image:Link;';

};
```

{% note warning %}
特别说明：
config.skin的皮肤需要自己进行下载，插件的skins目录下是没有。
有需要的同学可以邮件我：gaoming971366@163.com
{% endnote %}

#### 学习参考

官网配置：https://ckeditor.com/docs/ckeditor4/latest/api/CKEDITOR_config.html
CKEditor系列教程（一）：<a href="/articles/9743/" target="_blank" class="block_project_a">CkEditor的安装与基础使用</a>
CKEditor系列教程（二）：<a href="/articles/32150/" target="_blank" class="block_project_a">CkEditor的配置说明</a>
CKEditor系列教程（三）：<a href="/articles/32287/" target="_blank" class="block_project_a">CkEditor图片上传到服务端</a>
