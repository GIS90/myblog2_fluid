---
title: HTML+CSS标题艺术字
index_img: /img_index/index/20210105-001.jpg
categories:
  - [前端开发]
tags: [HTML, CSS]
abbrlink: 25985
date: 2021-01-05 21:32:19
updated: 2021-01-05 21:32:19
desc: HTML基于css开发的艺术字。
keywords: html, css, font
---

{% note success %}
HTML基于css开发的艺术字，有特效。
{% endnote %}



<!--more-->

<hr />

![](article_artfont.gif)

样式分为2种，其实就是把效果反过来了，pushdown与raisedoup。

#### HTML

```
<div class="f-pr-heading-wrapper">
  <a href="" class="f-pushdown-header f-pushdown-header-pushDown" title="HOVER ME">
      You can
      <span style="color:red;">search</span>
      everything
  </a>
</div>
```

#### CSS

f-pr-heading-wrapper为div基础标签。
background:-webkit-gradient是用于标签背景。

```
.f-pr-heading-wrapper {
    width: 100%;
    text-align: center;
    /*padding: 10px;*/
    background-color: #00BFFF;
	background:-webkit-gradient(linear, 0% 0%, 0% 100%,from(#FFFF00), to(#2EFE2E));
}

/* PUSH DOWN */
.f-pushdown-header {
    display: inline-block;
    text-align: center;
    font-family: 'Francois One', Helvetica, Arial, sans-serif;
    font-size: 6rem;
    color: #e7e7e7;
    text-decoration: none;
    transition: all 400ms ease-in-out;
}

.f-pushdown-header-pushDown {
    text-shadow: 1.5px 1.5px 0 #333, 0px 1.5px 0 #333, -1.5px -1.5px 0 #333, -1.5px -1.5px 0 #333, -1.5px 1.5px 0 #333, 1.5px -1.5px 0 #333, 0.77782px 0.77782px 0 #aaaaaa, 1.55563px 1.55563px 0 #aaaaaa, 2.33345px 2.33345px 0 #aaaaaa, 3.11127px 3.11127px 0 #aaaaaa, 3.88909px 3.88909px 0 #aaaaaa, 4.6669px 4.6669px 0 #aaaaaa, 5.44472px 5.44472px 0 #aaaaaa, 6.22254px 6.22254px 0 #aaaaaa, 7.00036px 7.00036px 0 #aaaaaa, 7.77817px 7.77817px 0 #aaaaaa;
}

.f-pushdown-header-pushDown:hover {
    color: #0d6aad;
    transform: translate(10px, 0);
    text-shadow: 1.5px 1.5px 0 #cac6c5, -1.5px -1.5px 0 #cac6c5, -1.5px -1.5px 0 #cac6c5, -1.5px 1.5px 0 #cac6c5, 1.5px -1.5px 0 #cac6c5;
}

/* RAISE UP */
.f-raiseup-header {
    display: inline-block;
    text-align: center;
    font-family: 'Francois One', Helvetica, Arial, sans-serif;
    font-size: 6rem;
    color: #e7e7e7;
    text-decoration: none;
    transition: all 400ms ease-in-out;
}

.f-raiseup-header-pushDown {
     text-shadow: 1.5px 1.5px 0 #cac6c5, -1.5px -1.5px 0 #cac6c5, -1.5px -1.5px 0 #cac6c5, -1.5px 1.5px 0 #cac6c5, 1.5px -1.5px 0 #cac6c5;
}

.f-raiseup-header-pushDown:hover {
    color: #0d6aad;
    transform: translate(10px, 0);
    text-shadow: 1.5px 1.5px 0 #333, 0px 1.5px 0 #333, -1.5px -1.5px 0 #333, -1.5px -1.5px 0 #333, -1.5px 1.5px 0 #333, 1.5px -1.5px 0 #333, 0.77782px 0.77782px 0 #aaaaaa, 1.55563px 1.55563px 0 #aaaaaa, 2.33345px 2.33345px 0 #aaaaaa, 3.11127px 3.11127px 0 #aaaaaa, 3.88909px 3.88909px 0 #aaaaaa, 4.6669px 4.6669px 0 #aaaaaa, 5.44472px 5.44472px 0 #aaaaaa, 6.22254px 6.22254px 0 #aaaaaa, 7.00036px 7.00036px 0 #aaaaaa, 7.77817px 7.77817px 0 #aaaaaa;

}
```
