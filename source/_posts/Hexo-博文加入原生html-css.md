---
title: Hexo博文加入原生html+css
index_img: /img_index/index/20190525-001.png
categories:
  - [Hexo]
tags: [Hexo, Hexo美化]
desc: Hexo博文加入原生html+css，可以让blog更加完美
keywords: hexo, next, Hexo, 美化, 插件, 博客, blog, html, css, 原生，相册
abbrlink: 5369
date: 2019-05-25 12:17:09
updated: 2019-06-02 12:17:09
---

#### 背景
{% note success %}
用markdown语法去写博文，***hexo g***会把md转为html，html可被浏览器识别渲染，就形成了大家看到的web。说实话，自学的我，虽然从事IT开发业2年多的经验了，但是要学习还有很多，之前写了一些关于在markdown中加入一些内置tags等方法使博文变得更加美观，可读。但是如果可以把原生html直接嵌入md文件中，这样写md文件就行云流水，而且样式还可以自定义，岂不美哉。
但是，事情往往哪会那么容易，走一步一个坑，花了几天时间才搞定嵌入，如果实现了的请勿看，本人小白，勿怪。
{% endnote %}

<!--more-->

<hr />

#### 正文

话不多说，直接看例子。

本人开发了一个相册sidebar功能，但是直接用markdown或者img标签去写，样式单一，就算使用**gp标签**，图片的样式也是很难看。于是把原生的html嵌入到md文件中，代码如下：
```

#### 2018-08-05 第一次吃牛排

第一次吃牛排

{% tabs 2018-07-27 乌镇之旅 %}
<!-- tab 点点滴滴 -->
<div class="phote-page">
	<div class="phote-list">
		<div class="phote-column">
			<img src="images/fristniupai/1.jpg">
			<img src="images/fristniupai/5.jpg">
			<img src="images/fristniupai/9.jpg">
		</div>
		<div class="phote-column">
			<img src="images/fristniupai/8.jpg">
            <img src="images/fristniupai/2.jpg">
            <img src="images/fristniupai/6.jpg">
		</div>
		<div class="phote-column">
			<img src="images/fristniupai/3.jpg">
            <img src="images/fristniupai/7.jpg">
            <img src="images/fristniupai/11.jpg">
		</div>
        <div class="phote-column">
            <img src="images/fristniupai/4.jpg">
            <img src="images/fristniupai/10.jpg">
        </div>
	</div>
</div>
<!-- endtab -->
{% endtabs %}
```

写完md文件之后，查看页面，发现还是难看，因为没有加入样式，一般都嵌入html的地方都是指定的开发部分，所有给div标签命名一个id最合适，方便写css样式，把个性化的css的样式写入自动样式文件：blog/theme/next/source/css/_custom/custom.styl，增加对应的代码：
```
// 相册自定义样式
.phote-list {
	display: flex;
	flex-direction: row;
	flex-wrap: nowrap;
	align-items: flex-start;
}
.phote-list img {
    border-radius: 35px;
    // width: 320px;
    // height: 230px;
    padding: 6px;
    background-color: #909497;
    margin: 0px 0px 0px 0px;
    box-shadow: 15px 15px 15px rgba(50, 50, 50, 0.99);
    transition: all 1s ease-in;
}
.phote-list img:hover {
    box-shadow: 35px 35px 35px rgba(50, 50, 50, 0.8);
    transform: rotate(0deg) scale(1.6);
    background-color: black;
    padding: 15px;
    z-index: top;
}
```
在css样式中，我对图片加了一个鼠标滑过放大的动画，既然可以自定义样式，加什么特效都随你了。

最后，重启服务***hexo g && Hexo s***，刷新页面就会看到效果。

#### 问题

> 引入原生的html标签，发现***hexo g***之后，在生成的.html页面，在**div标签**会自动加入**br标签**，导致有些特效出现问题

{% note info %}
<font size="4" color="red">**解决方案：**</font>
在加入的原生的html部分用raw-endraw内置tags包起来就可以，示例代码：
{% code %}
{% raw %}
<div class="timeline">
    <div class="timeline-item" date-is='1997/07 ~ 2013/07'>
        <div>上学阶段</div>
        <p>
            书中自有黄金屋，书中自有颜如玉
        </p>
    </div>
    <div class="timeline-item" date-is='2014/03 ~ 2016/06'>
        <div>盲目阶段</div>
        <p>
            一艘没有航行目标的船
        </p>
    </div>
    <div class="timeline-item" date-is='2016/07 ~ 至今'>
        <div>追求阶段</div>
        <p>
            生活的理想就是理想的生活
        </p>
    </div>
</div>
{% endraw %}
{% endcode %}
{% endnote %}

#### 坑

一开始趟坑的时候，天真的我居然以为可以把原生的html直接粘贴复制到md文件中就可以了。。。。。。结果，哈哈我真是太天真了。不过后来尝试把样式放到自定义文件中，把样式与标签分离，没想到成功了。不管怎么样，记录下来，希望能帮到他人。
