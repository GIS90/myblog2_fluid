---
title: Hexo如何引用自定义js代码
index_img: /img_index/index/20190603-001.jpg
categories:
  - [Hexo]
tags: [Hexo, Hexo美化]
top: false
abbrlink: 41680
date: 2019-06-03 11:28:23
updated: 2019-06-03 11:28:23
desc: Hexo搭建的blog如何引用自定义js代码
keywords: hexo, next, Hexo, 美化, 插件, 博客, blog, js, 自定义
---

### 背景
{% note success %}
上个月研究Hexo如何引用原生的html+css，而且也写了一篇文章发表。但是如果想把自己的博客做的更加突出、像模像样，没有js代码去实现功能是不行的，正好想做一个主题切换功能，借着这个功能介绍一下Hexo是如何引用js代码的。
{% endnote %}

<!--more-->
<hr />

### 正文

先讲述怎么引用js代码的，完了在通过实战验证。

#### 引用方式

Hexo+Next通过渲染模板（.swig）以及md文件生成对应的html页面，那么问题就来了，既然是模板，肯定会有一个base文件，这里就不做详述了，直接说：**blog/themes/next/layout/_layout.swig**。
打开这个文件，会发现里面整个页面渲染的框架以及对第三方文件的引用，看下引用js文件代码：
```
{% include '_scripts/next-boot.swig' %}
{% include '_scripts/scroll-cookie.swig' %}
{% include '_scripts/exturl.swig' %}
{% include '_third-party/quicklink.swig' %}
{% include '_third-party/comments/index.swig' %}
{% include '_third-party/search/index.swig' %}
{% include '_third-party/analytics/lean-analytics.swig' %}
{% include '_third-party/analytics/firestore.swig' %}
{% include '_third-party/math/index.swig' %}
{% include '_third-party/pdf.swig' %}
{% include '_third-party/mermaid.swig' %}
{% include '_third-party/baidu-push.swig' %}
{% include '_third-party/schedule.swig' %}
{% include '_third-party/needsharebutton.swig' %}
{% include '_third-party/rating.swig' %}
{% include '_third-party/pangu.swig' %}
{% include '_third-party/bookmark.swig' %}
{% include '_third-party/copy-code.swig' %}
{% include '_third-party/chatra.swig' %}
{% include '_third-party/tidio.swig' %}

<script type="text/javascript" src="/js/src/click_magic.js"></script>
<script type="text/javascript" src="/js/src/theme_change.js"></script>
```
还是通过传统的方式引用js代码，相对应的**script**直接引用的是js文件，而**include**引用的是swig文件，来点代码看下swig文件里面写的是啥，例子我取的是**blog/themes/next/layout/post.swig**：
```
{% extends '_layout.swig' %}
{% import '_macro/post.swig' as post_template %}
{% import '_macro/sidebar.swig' as sidebar_template %}

{% block title %}{{ page.title }} | {{ title }}{% endblock %}

{% block page_class %}page-post-detail{% endblock %}

{% block content %}

  <div id="posts" class="posts-expand">
    {{ post_template.render(page) }}
  </div>

{% endblock %}

{% block sidebar %}
  {{ sidebar_template.render(true) }}
{% endblock %}

{% block script_extra %}
  {% include '_scripts/pages/post-details.swig' %}
{% endblock %}
```
里面存放的是html（标签+css）+一些next主题带的语法。
说了这么多，到底咋搞。。。。只需要2步：
{% note info %}
- 在**blog/themes/next/source/js/src**目录下建立一个js文件，名称自取。
- 在**blog/themes/next/layout/_layout.swig**文件中通过**script**引用自定义的js文件。
{% endnote %}

#### 实战

通过一个切换主题的功能来实现上述。

- 首先，在自定义的header文件：**blog/themes/next/layout/_custom/header.swig**，加入一个button，代码如下：
```
<button id="theme_change"
        class="theme_change"
        type="button"
        onclick="theme_change()"
>
    暗黑系
</button>
```
- 接下来定义button按钮的样式，打开自定义样式文件：**blog/themes/next/source/css/_custom/custom.styl**，加入一下代码：
```
// 功能按钮样式
.theme_change {
    width: 160px;
    height: 37px;
    border-radius: 50px 50px 50px 50px;
    color: white;
    font-size: 16px;
    background-color: #333333;
    margin-left: 40px;
    margin-bottom: 20px;
    border: 3px solid red;
}
```
- 在刚才说的自定义js代码目录下新建一个theme_change.js文件，加入一下代码：
```
// 主题切换功能
function theme_change() {
    // 切换字内容
    var button = document.getElementById('theme_change');
    var pattern = new RegExp('暗黑系', 'i');
    if (pattern.test(button.innerHTML)){
        button.innerHTML = "光明系";
    } else{
        button.innerHTML = "暗黑系";
    }
    // 切换主题
    document.body.classList.toggle('dark-theme');
};
```
- 编写暗黑主题的样式，还是自定义样式文件，加入一下代码：
```
// 自制主题切换功能样式
body.dark-theme {
    background: #282c34
}
```
- 最后一步，自定义js文件的引用。打开**blog/themes/next/layout/_layout.swig**文件，在底部**结束body标签**上面引用，代码：
```
<script type="text/javascript" src="/js/src/theme_change.js"></script>
```
- ***hexo g && hexo s***

不出意外，会有一个简单的主题切换功能，样式我也是简单改变了一下背景色，有兴趣的同学可以编写2套Next主题自定义样式。

{% note info %}
### 问题
切换主题的功能有个问题，只能改变当前page的主题，切换新的page之后，恢复到初始化主题，没有后台管理的原因记录不了当前的主题状态，待解决。
{% endnote%}

### 结束

<font color="red" size="5">***既然做，不说要一定做到最好，起码不能糊弄，与其糊弄还不如不做！！！***</font>
