---
title: Hexo+Next搜索优化（一）google搜索
index_img: /img_index/index/20190303-001.jpg
desc: 关于博客SEO优化系列的教程，本篇关于在google搜索优化(新版Search Console)
categories:
  - [seo优化]
  - [Hexo]
tags: [Hexo, seo优化, Hexo插件]
keywords: hexo, next, Hexo, 美化, 插件, 博客, blog, seo, google, 搜索, SearchConsole
abbrlink: 42646
date: 2019-03-03 21:33:59
updated: 2019-11-03 21:33:59
---

{% label default@Hexo %} {% label danger@SEO优化系列 %} {% label success@google %} 

### 背景
{% note primary %}
博客发布有一段时间了，而且也通过github提供的便利发布到公网上，但是除了直接域名访问，本人没有花钱买域名，只用了一个github免费提供的一个，在google居然搜索不到，只能优化一下网站在google的SEO。
{%  endnote %}

<!--more-->

<hr />

### 正文
本文讲解实现对google对网站的搜索优化，<font size="3" color="red">Hexo+Next搜索优化教程第一篇</font>。

1. #### 安装hexo-generator-sitemap
在blog根目录，执行一下命令：
```
npm install hexo-generator-sitemap --save
```

2. #### 生成sitemap.xml文件
- 第一：找到blog配置文件：***blog/_config.yml***，搜索<font size="4" color="red">***url***</font>，把地址改成自己的网站地址。
```
url: https://gis90.github.io
```
- 第二：把下列代码追加到打开的配置文件末尾。
```
sitemap:
  path: sitemap.xml
```
- 第三：***hexo g***重新生成文件，在blog/public目录下会生成一个***sitemap.xml***文件。

3. #### 添加蜘蛛协议
在blog/source目录下新建一个robots.txt文件，内容如下：
```
User-agent: *
Allow: /
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
```
解释一下：***Allow***字段的值即为允许搜索引擎爬区的内容，可以对应到主题配置文件中的目录配置，如果菜单栏还有其他选项都可以按照格式自行添加；***Disallow***就是不允许搜索引擎爬区的内容，可以把网站相关的一些js、等资源写入。

4. #### 更新github.io
把新生成的sitemap.xml&&robots.txt上传到github。
```
hexo g -d
```

5. #### google站点管理
    {% note info %}
    旧版：
    打开***Search Console***：[旧版首页](https://www.google.com/webmasters/tools/home?hl=zh-CN)，添加属性。

  {% endnote %}

  ![](seo_google_sc_main.png)

- 添加资源
***Search Console***出新的版本了，在使用过程中，一直提示转到新版，那就用新版进行讲解，点击页面上的***使用新版Search Console***。新版不知道怎么添加地址的请查看下列图片，在<font size="4" color="red">左上角搜索资源->添加资源</font>。
![](seo_google_sc_new.png)
- 选择资源
![](seo_google_sc_select.png)

5. #### 站点验证
进入站点验证，我在这里验证了2种方式：HTML文件&&HTML标记，怎么验证官方都有说明，按照说明做即可，我这里进行简述一下。
![](seo_google_sc_check.png)

- 方式一
打开文件：blog/themes/hexo-theme-next/layout/_partials/head/head.swig，添加代码到文件第二行，方便进行加载：
```
<meta charset="UTF-8"/>
<meta name="google-site-verification" content="nOQH_Lr6zdiXCxCLRslnRmrnULAd7XsMJc-3MQo0iMI" />
```
- 方式二
下载googole验证文件：google6377d5ca65812ad1.html，把文件放到blog/public目录下。
完成上述操作之后，执行***hexo g -d***，把验证部门上传到github，密钥与密钥文件都是本人网站的，替换google提供的密钥即可。

6. #### robots测试（旧版）
在新版***Search Console***中没有发现**robots.txt测试工具**功能，只能切换到老版，在新版的左下角有个**转到旧版**功能，点击**抓取->robots.txt测试工具**。在下面可以输入自己网站的一些网址地址，看是否可以测试通过。
![](seo_google_sc_robots.png)

{% note info %}
新版中没了此功能，也许就是不需要此步验证，新版***Search Console***也在研究中，有问题的可以留言给我一起交流。
{% endnote %}

7. #### 提交站点地图
- 测试sitemap.xml地址
访问：https://gis90.github.io/sitemap.xml
测试sitemap.xml可被访问到。
- Search Console提交
点击左侧***站点地图***，在空白处输入***sitemap.xml***，点击提交，如果有错误去fix就好，不过一般情况下是不会出错的，我的有error是因为我的有一个博文不知道为何在***updated: 22019-03-24 20:44:08***，时间上出现了问题，导致有问题。
![](seo_google_sc_sitemap.png)

8. #### 等待结果
点击概述/效果等功能，提示**正在处理数据，请过几天再来查看**，现在能做的只是等待结果出来在进行下一步的优化了。
![](seo_google_sc_result.png)

9. #### 更新结果

    这里展示了近3个月的结果：
    > 访问量

    ![](fangwenqingkuang.png)
    > 覆盖率

    ![](fugailv.png)
    > 地区

    ![](diqu.png)

### 相关链接
- search console：https://www.google.com/webmasters/tools/home?hl=zh-CN
- 所有权验证：https://support.google.com/webmasters/answer/9008080
