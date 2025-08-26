---
title: HTML+CSS+HR分割线
index_img: /img_index/index/20210103-001.png
categories:
  - [前端开发]
tags: [HTML, CSS]
abbrlink: 50666
date: 2021-01-03 19:32:05
updated: 2021-01-03 19:32:05
desc: 推荐几款好看的hr标签分割线，基于单纯的html与css，也是网上找的，总结一下。
keywords: html, css, hr
---

{% note success %}
推荐几款好看的hr标签分割线，也是网上找的，基于html与css，总结一下。
{% endnote %}



<!--more-->

![](article_hr.png)

#### HTML

```
<hr class="hr-grow-base hr-grow-trans hr-grow-hr1">
内容。。。。。。。。。。。。。。。。。。。。
<hr class="hr-grow-base hr-grow-trans hr-grow-hr2">
<hr class="hr-color-fade">
<hr class="hr-lean-line">
<hr class="hr-gray-fade">
<hr class="hr-grap-shadow">
<hr class="hr-dots">
<hr class="hr-accessory">
<hr class="hr-pill">
<hr class="hr-vertical-lines">
<hr class="hr-slash">
<hr class="hr-wave">
<hr class="hr-stars">
```
其中，关于Hr grow style样式需要说明一下，除了加上html与css之外，还需要加上js才会有动画效果。
```
setTimeout(function(){
    $('.hr-grow-trans').addClass('hr-grow-add');
}, 275);
```

#### CSS

```
/* Color fade */
.hr-color-fade {
    width: 100%;
    margin: 0 auto;
    border: 0;
    height: 4px;
    background: #333;
    background-image: linear-gradient(to right, red, #333, rgb(9, 206, 91));
}

/* Gray fade */
.hr-gray-fade {
    width: 100%;
    margin:0 auto;
    border: 0;
    height: 4px;
    background-image: linear-gradient(to right, rgba(0, 0, 0, 0), rgba(0, 0, 0, 0.75), rgba(0, 0, 0, 0));
}

/*Lean line*/
.hr-lean-line {
  position: relative;
  display: block;
  margin-top: 4em;
  margin-bottom: 4em;
  height: 3px;
  border:none;
  background: linear-gradient(to right, transparent 50%, #fff 50%), linear-gradient(to right, #00b9ff, #59d941);
  background-size: 1.5rem, 100%;
  transform: rotate(-4.5deg);
  transform-origin: 50% 0;
}

/*Grap-shadow*/
.hr-grap-shadow {
    height: 10px;
    border: 0;
    box-shadow: 0 10px 10px -10px #3c3f41 inset;
}


/* Hr grow style
html:
<hr class="hr-grow-base hr-grow-trans hr-grow-hr1">
<hr class="hr-grow-base hr-grow-trans hr-grow-hr2">

js:
setTimeout(function(){
    $('.hr-grow-trans').addClass('hr-grow-add');
}, 275);
*/

.hr-grow-base{
    margin-top: 20px;
    padding: 1.5px 0;
    border: none;
    /*background-color: rgb(250, 150, 0);*/
    background-image: linear-gradient(to right, red, #333, rgb(9, 206, 91));
    letter-spacing: 5px;
}
.hr-grow-hr1{
    margin-left: 2%;
}
.hr-grow-hr2{
    margin-right: 2%;
}
.hr-grow-trans{
    -webkit-transition: width 1s ease-out;
    transition: width 1s  ease-out;
    width : 0;
}
.hr-grow-add{
    width: 96%;
}

/*Dots*/
.hr-dots{
    color: orange;
    border-width: 0 0 8px;
    border-style: solid;
    border-image: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 2 1" width="8" height="4"><circle fill="orange" cx="1" cy="0.5" r="0.5"/></svg>') 0 0 100% repeat;
    width: 90%;
}

/*accessory*/
.hr-accessory {
    height: 6px;
    background-image: radial-gradient(closest-side, gray, rgba(128, 128, 128, 0) 100%);
    position: relative;
}

.hr-accessory:after {
    position: absolute;
    top: 50%;
    left: 50%;
    display: block;
    background-color: #bfbfbf;
    height: 12px;
    width: 12px;
    transform: rotate(45deg);
    margin-top: -10px;
    margin-left: -10px;
    border-radius: 4px 0;
    border: 4px solid rgba(255, 255, 255, 0.35);
    background-clip: padding-box;
    box-shadow: -10px 10px 0 rgba(255, 255, 255, 0.15), 10px -10px 0 rgba(255, 255, 255, 0.15);
}


/*pill*/
.hr-pill {
    height: 1rem;
    border-radius: 1rem;
    color: teal;
    background-color: #00FFFF;
    border: 2px solid currentColor;
    width: 80%;
}


/*vertical-lines*/
.hr-vertical-lines {
    height: 1rem;
    color: orange;
    background-image: linear-gradient(90deg, currentColor, currentColor 33.33%, transparent 33.33%, transparent 100%);
    background-size: 3px 100%;
    width: 80%;
}

/*slash*/
.hr-slash {
    height: 10px;
    background-image: linear-gradient(45deg, rgba(13, 13, 13, 0), rgba(13, 13, 13, 0) 33.33%, #0d0d0d 33.33%, #0d0d0d 66.67%, rgba(13, 13, 13, 0) 66.67%, rgba(13, 13, 13, 0) 100%);
    background-size: 10px 100%;
    width: 90%;
}


/* Wave */
.hr-wave {
    width: 96%;
    border-image: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 12 15" width="12" height="15"><path fill="none" stroke="rgba(191, 191, 191, 0.9)" stroke-width="3" d="M0,13.5c3,0,3-12,6-12s3,12,6,12"/></svg>') 0 0 100% repeat;
    border-width: 0 0 0.8rem;
    border-style: solid;
}

/* stars */
.hr-stars {
    border: 0;
    height: auto;
    color: gold;
    text-align: center;
}
.hr-stars:after {
    content: "★";
    font-size: 1em;
    text-shadow: -6em 0, -5em 0, -4em 0, -3em 0, -2em 0, -1em 0, 1em 0, 2em 0, 3em 0, 4em 0, 5em 0, 6em 0;
}
```
