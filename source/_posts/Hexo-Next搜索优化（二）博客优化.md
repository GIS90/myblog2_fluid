---
title: Hexo+Next搜索优化（二）博客优化
index_img: /img_index/index/20190317-001.jpg
categories:
  - [Hexo]
tags: [Hexo, seo优化]
desc: 关于博客SEO优化系列的教程，本篇关于blog内在的优化
keywords: hexo, next, Hexo, 美化, 插件, 博客, blog, seo, google, 搜索, baidu
abbrlink: 62177
date: 2019-03-17 19:44:22
updated: 2019-08-17 19:44:22
---

{% label danger@Hexo %} {% label info@SEO优化系列 %} {% label success@博客优化 %} 

### 背景
{% note primary %}
前几天写了一篇关于博客在google搜索引擎上的优化，发现通过***gis90 + 标题***，还是可以搜出来的，但是直接通过标题去🔍搜索，还是搜不到什么东西，还得继续优化啊。
{%  endnote %}


<!--more-->
<hr />

上网继续查资料，整理到一起。

### 正文

本文总结了主要从文章的关键字、url、配置等方面进行优化，提高搜索度。

> #### 关键字

打开每一篇博客，增加***keywords、desc***，增加文章的搜索匹配，代码如下：
```
---
title: Hexo+Next搜索优化（二）博客优化
comments: false
categories:
  - [seo优化]
  - [Hexo]
tags: [Hexo, seo优化]
top: false
date: 2019-04-27 19:44:22
updated: 2019-04-27 19:44:22
desc: Hexo+Next搜索优化（二）博客优化
keywords: hexo, seo, google, 搜索
---
```
{% note info %}
<font size="4">**附加**</font>
- 可以打开**blog/scaffolds/post.md**文件，修改代码如下，一劳永逸。

{% code %}
---
title: {{ title }}
desc:
date: {{ date }}
updated: {{ date }}
comments: false
categories:
  - []
tags: []
keywords:
top: false
---
{% endcode %}
- 不要用***description***，会报错，可能是版本问题，我用的是***Hexo（v3.8.0）+ Next（v7.0.1）***。
{% endnote%}

> #### URL

打开Hexo的配置文件：**blog/_config.yml**，搜索<font size="4" color="red">***permalink***</font>，更改代码如下：
```
# permalink: :year/:month/:day/:title/
permalink: :category/:title/
```
{% note info %}
<font size="4">**解释**</font>
不更新配置之前，文章的默认链接一个四级url，形式是**:year/:month/:day/:title/**，而且title会导致url过长，用**:category/:title/**代替原配置，url的资源只有2级，对于搜索引擎会更加有好一些。
{% endnote%}

> #### Hexo配置

打开Hexo的配置文件：**blog/_config.yml**，搜索<font size="4" color="red">***subtitle***</font>，补全站点的基本信息配置。
```
title:  #标题
subtitle:  #子标题
description: #描述
url: #url
```
> #### Next配置

打开Next的配置文件：**blog/themes/next/_config.yml**，搜索<font size="4" color="red">***SEO Settings***</font>，更改配置如下：
```
canonical: true
seo: true
index_with_subtitle: false
baidu_push: true
```
具体的对应什么含义，自行查看配置文件解释。
{% note info %}
特别说明：index_with_subtitle
在站点index所有页，是否显示Hexo站点设置的副标题，我觉得没什么作用，还影响样式，这个在我的配置中设置了false
{% endnote %}

> #### 首页标题优化Title

打开Next的指定文件：**blog/themes/next/layout/index.swig**，更改如下：
```
{% block title %} {{ config.title }} {% endblock %}
```
改成
```
{% block title %} {{ config.title }} - {{ config.keywords }} - {{ config.description }} {% endblock %}
```
{% note info %}
<font size="4">**解释**</font>
首页将文章的关键字、以及描述，增大搜索字的匹配率，这个只针对于首页，记住是首页、首页、首页。
{% endnote%}

> #### 每篇博文title、keywords、desc

打开Next的指定文件：**blog/themes/next/layout/_partials/head/head.swig**，添加一下如下：
```
<!-- title、keywords、desc关键字 -->
{% if page.title %}
  <title>{{ page.title }}</title>
  <meta name="title" content="{{ page.title }}" />
{% endif %}
{% if page.keywords %}
  <meta name="keywords" content="{{ page.keywords }}" />
{% elif page.tags and page.tags.length %}
  <meta name="keywords" content="{% for tag in page.tags %}{{ tag.name }},{% endfor %}" />
{% elif theme.keywords %}
  <meta name="keywords" content="{{ theme.keywords }}" />
{% endif %}
{% if page.desc %}
  <meta name="description" content="{{ page.desc }}" />
{% endif %}
```
{% note info %}
<font size="4">**解释**</font>
将增加每篇博文设置的title、keywords、desc，SEO优化重点之一，增大搜索字的匹配率。
{% endnote%}

> #### 添加robot.txt

在blog/public目录新建一个文件robot.txt，进行配置一下内容：
```
User-agent: *
Allow: /
Allow: /articles/
Allow: /archives/
Allow: /categories/
Allow: /tags/
Allow: /messagepad/
Allow: /resources/
Allow: /about/
Disallow: /vendors/
Disallow: /js/
Disallow: /css/
Disallow: /fonts/
Disallow: /vendors/
Disallow: /fancybox/

Sitemap: https://gis90.github.io/sitemap.xml
Sitemap: https://gis90.github.io/baidusitemap.xml
```

写上你常用的遍历url、以及sitemap地址。

> #### 图片压缩

在博客中关于使用的图片，几乎都进行压缩处理，我使用的是在线压缩：https://www.yasuotu.com/。

> #### 文件压缩

使用***gulp***进行文件压缩，本人是搞后端的，参考别人seo优化的时候，也是看了一下***glup***的用法，记住：<font size=5.5 color='blue'>glup用来处理静态资源的一个工具。</font>

下面介绍一下具体的实现方法：
- 安装glue
```
npm install gulp@3.9.1 -g
npm install gulp-minify-css gulp-uglify gulp-htmlmin gulp-htmlclean gulp --save
npm install gulp-concat
npm install gulp-imagemin
```
- 在blog文件夹下创建gulpfile.js，加入一下代码：
```
var gulp = require('gulp');
var minifycss = require('gulp-minify-css');
var uglify = require('gulp-uglify');
var htmlmin = require('gulp-htmlmin');
var htmlclean = require('gulp-htmlclean');
var imagemin = require('gulp-imagemin');

// 压缩html
gulp.task('minify-html', function() {
    return gulp.src('./public/article/**/*.html')
        .pipe(htmlclean())
        .pipe(htmlmin({
            removeComments: true,
            minifyJS: true,
            minifyCSS: true,
            minifyURLs: true,
        }))
        .pipe(gulp.dest('./public'))
});
// 压缩css
gulp.task('minify-css', function() {
    return gulp.src('./public/**/*.css')
        .pipe(minifycss({
            compatibility: 'ie8'
        }))
        .pipe(gulp.dest('./public'));
});
// 压缩js
gulp.task('minify-js', function() {
    return gulp.src(['./public/js/**/.js','!./public/js/**/*min.js'])
        .pipe(uglify())
        .pipe(gulp.dest('./public'));
});
// 压缩图片
gulp.task('minify-images', function() {
    return gulp.src('./public/images/*.*')
        .pipe(imagemin(
        [imagemin.gifsicle({'optimizationLevel': 3}),
        imagemin.jpegtran({'progressive': true}),
        imagemin.optipng({'optimizationLevel': 7}),
        imagemin.svgo()],
        {'verbose': true}))
        .pipe(gulp.dest('./public/images'))
});
// 默认任务
gulp.task('default', [
    'minify-html','minify-css','minify-js','minify-images'
]);
```
- 运行命令
```
glue
```
在控制台会看见压缩的过程，整个压缩过程在图片压缩处理起来有点慢，耐心等待。

### 学习

在优化过程中，参考了一些人的博客以及官方文档，感谢各位大神。

参考一：https://github.com/theme-next/hexo-theme-next/issues/866
参考二：https://hjptriplebee.github.io/hexo%E7%9A%84SEO%E6%96%B9%E6%B3%95.html/
参考三：https://www.greateman.top/Next%E4%B8%BB%E9%A2%98SEO%E4%BC%98%E5%8C%96.html
参考四：http://www.ehcoo.com/seo.html
glup：https://www.jianshu.com/p/87a773a81dbd

{% note info %}
<font size="4">**疑问**</font>
- 针对于Next主题的配置文件选项：**index_with_subtitle**，配置文件的解释： If true, will add site-subtitle to index page, added in main hexo config。
如果文章没有subtitle，那么这个配置是不是就无用了。
{% endnote%}

### 拓展

看了几个人的博客发现在改Title方面参数不一致，也搞不懂应该参考哪个，于是自己把参数每个试了一次，参数如下：
```
{{ title }}
{{ keywords }}
{{ desc }}
{{ description }}

{{ theme.keywords }}    # 可用
{{ config.title }}  # 可用
{{ theme.description }} # 可用
```
很简单，其实就是把这些参数写在一个博文md文件里面，在生成的html页面有哪些是有值的，这是我的测试方法，结果可想而知，如有不对，欢迎留言进行交流。
