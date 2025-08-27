---
title: hexo添加鼠标点击效果
index_img: /img_index/index/20190108-001.png
desc: hexo搭建的博客添加鼠标点击效果，程序员的语言 && 红心效果
categories:
  - [Hexo]
tags: [Hexo, Hexo美化]
keywords: hexo, next, Hexo, 美化, 插件, 博客, blog, 点击, 鼠标
abbrlink: 37795
date: 2019-01-08 18:25:57
updated: 2019-07-08 18:25:57
---


{% note success %}
博文中鼠标点击出现❤️&&文字特效，<font color="red" size="5">***持续更新中～～～***</font>
{% endnote %}

<!--more-->
<hr />

![](article_click.gif)
特效比较简单，只需要写好js特效文件加载就OK了。

> 红心

***/themes/next/source/js/src***目录下新建文件click_love.js，代码如下：
```
!function(e,t,a){function n(){c(".heart{width: 10px;height: 10px;position: fixed;background: #f00;transform: rotate(45deg);-webkit-transform: rotate(45deg);-moz-transform: rotate(45deg);}.heart:after,.heart:before{content: '';width: inherit;height: inherit;background: inherit;border-radius: 50%;-webkit-border-radius: 50%;-moz-border-radius: 50%;position: fixed;}.heart:after{top: -5px;}.heart:before{left: -5px;}"),o(),r()}function r(){for(var e=0;e<d.length;e++)d[e].alpha<=0?(t.body.removeChild(d[e].el),d.splice(e,1)):(d[e].y--,d[e].scale+=.004,d[e].alpha-=.013,d[e].el.style.cssText="left:"+d[e].x+"px;top:"+d[e].y+"px;opacity:"+d[e].alpha+";transform:scale("+d[e].scale+","+d[e].scale+") rotate(45deg);background:"+d[e].color+";z-index:99999");requestAnimationFrame(r)}function o(){var t="function"==typeof e.onclick&&e.onclick;e.onclick=function(e){t&&t(),i(e)}}function i(e){var a=t.createElement("div");a.className="heart",d.push({el:a,x:e.clientX-5,y:e.clientY-5,scale:1,alpha:1,color:s()}),t.body.appendChild(a)}function c(e){var a=t.createElement("style");a.type="text/css";try{a.appendChild(t.createTextNode(e))}catch(t){a.styleSheet.cssText=e}t.getElementsByTagName("head")[0].appendChild(a)}function s(){return"rgb("+~~(255*Math.random())+","+~~(255*Math.random())+","+~~(255*Math.random())+")"}var d=[];e.requestAnimationFrame=function(){return e.requestAnimationFrame||e.webkitRequestAnimationFrame||e.mozRequestAnimationFrame||e.oRequestAnimationFrame||e.msRequestAnimationFrame||function(e){setTimeout(e,1e3/60)}}(),n()}(window,document);
```

> 文字

***/themes/next/source/js/src***目录下新建文件click_magic.js，代码如下：
```
/* 鼠标特效 */
var a_idx = 0;
jQuery(document).ready(function($) {
   $("body").click(function(e) {
       var a = new Array("Python", "Java", "Go", "C", "C++", "C#", "JavaScript" ,
       "Php", "Sql", "R");
       var $i = $("<span/>").text(a[a_idx]);
       a_idx = (a_idx + 1) % a.length;
       var x = e.pageX,
       y = e.pageY;
       $i.css({
           "z-index": 9999,
           "top": y - 20,
           "left": x,
           "position": "absolute",
           "font-weight": "bold",
           "color": "#ff6651"
       });
       $("body").append($i);
       $i.animate({
           "top": y - 180,
           "opacity": 0
       },
       1500,
       function() {
           $i.remove();
       });
   });
});
```

建立好js文件之后，打开文件***/themes/next/layout/_layout.swig***，搜索***</body>***，加入代码：
```
<script type="text/javascript" src="/js/src/click_love.js"></script>
<script type="text/javascript" src="/js/src/click_magic.js"></script>
```
完成上述操作hexo g && hexo s就可以看到效果啦，如果只要一种特效，保留一个js文件即可。


<font size=6.5 color='red'>鼠标点击特效持续更新中。。。。。。</font>
