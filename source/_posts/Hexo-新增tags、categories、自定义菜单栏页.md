---
title: Hexo新增tags、categories、自定义菜单栏页
index_img: /img_index/index/20181028-001.jpg
categories:
  - [Hexo]
tags: [Hexo]
desc: Hexo新增tags、categories、about、自定义菜单栏页
keywords: hexo, next, Hexo, 美化, 插件, 博客, blog, tags, categories, 菜单栏, 自定义
abbrlink: 33202
date: 2018-10-28 00:16:19
updated: 2018-10-28 00:16:19
---


### 背景
{% note success %}
Hexo初始化博客之后，主题选用了人气最高的Next，但是发现菜单栏默认是不自动添加tags页与categories页，查资料，终于创建成功，记录下来。
{% endnote %}

<!--more-->
<hr />

### 正文

除了讲述tags、categories、about自带的自动化页面，还有讲述自定义sidebar页面，方便用于自己定义相册、等自己喜欢的页面。

1. #### tags

    - new page
    在blog根目录，执行下列命令：
    ```
    hexo new page tags
    ```
    不出意外会自动生成文件blog/source/tags/index.md，目前位置没有出现过任何问题。有问题的同学可以留言给我。
    - 配置
    打开blog/source/tags/index.md，默认是下列内容：
    ```
    ---
    title: 标签
    date: 2019-10-22 14:22:08
    ---
    ```
    改成
    ```
    ---
    title: 标签
    date: 2019-10-22 13:58:44
    type: "tags"
    comments: false
    ---
    ```
    - 添加tags
    给文章添加对应的tags，打开一篇文件，例子如下：
    ```
    ---
    title: Hexo+Next搭建属于自己的Blog
    comments: false
    date: 2018-10-22 23:03:42
    updated: 2019-10-22 23:03:42
    desc: Hexo+Next搭建属于自己的Blog
    categories:
      - [Hexo]
    tags: [Hexo]
    keywords: hexo, next, blog
    ---
    ```
    - 重启server
    重新生成静态页面以及开启服务，之后刷新页面就会看到。
    ```
    hexo g
    hexo s
    ```

2. #### categories

    categories页创建与tags页操作一样。
    - new page
    在blog根目录，执行下列命令：
    ```
    hexo new page categories
    ```
    - 配置
    打开blog/source/categories/index.md，默认是下列内容：
    ```
    ---
    title: 分类
    date: 2018-10-22 14:25:08
    ---
    ```
    改成
    ```
    ---
    title: 分类
    date: 2018-10-22 14:25:08
    type: "categories"
    comments: false
    ---
    ```
    - 添加categories
    给文章添加对应的categories：
    ```
    ---
    title: Hexo+Next搭建属于自己的Blog
    comments: false
    date: 2018-10-22 23:03:42
    updated: 2019-10-22 23:03:42
    desc: Hexo+Next搭建属于自己的Blog
    categories:
      - [Hexo]
    tags: [Hexo]
    keywords: hexo, next, blog
    ---
    ```
    - 重启server
    同样重启服务。
    ```
    hexo g && hexo s
    ```

3. #### 指定分类列表页

    这个功能主要用于把已经分类的自分类单独形成一个页面放在sidebar，打开之后是一个文章list效果，建议1～2个。
    ![](enjoy_list.png)
    - 子分类
    找到需要在sidebar显示的子分类，中文、英文都可以。
    - 配置
    打开Next主题配置文件：blog/theme/next/_config.yml，搜索<font size="4" color="red">***menu***</font>，更改代码如下：
    ```
    menu:
      home: / || home
      archives: /archives/ || history
      tags: /tags/ || tags
      categories: /categories/ || list
      movie: /categories/影视说/ || film
      enjoy: /categories/娱乐/ || battery-full
      messagepad: /messagepad/ || sticky-note
      about: /about/ || user
      # schedule: /schedule/ || calendar
      # sitemap: /sitemap.xml || sitemap
      # commonweal: /404/ || heartbeat
    ```
    设置的标准：
    第一列 / themes/next/languages/zh-Hans.yml 对应的关键字
    第二列 / url 路径
    第三列 / 图标 https://fontawesome.com/cheatsheet?from=io
    - 添加关键字
    打开文件：blog/themes/next/languages/zh-Hans.yml，添加对应的mapping：
    ```
    menu:
      home: 首页
      archives: 时间轴
      categories: 分类
      tags: 标签
      about: 关于
      search: 搜索
      schedule: 日程表
      sitemap: 站点地图
      commonweal: 公益 404
      movie: 影视说
      messagepad: 留言板
      enjoy: 娱乐
    ```
    - 重启server
    同样重启服务。
    ```
    hexo g && hexo s
    ```

4. #### about

    about是sidebar自我介绍的一栏，很重要，但是生成的方式与tags、categories如出一辙。
    - new page
    在blog根目录，执行下列命令：
    ```
    hexo new page about
    ```
    - 配置
    打开blog/source/about/index.md，默认是下列内容：
    ```
    ---
    title: about
    date: 2018-10-22 14:35:08
    ---
    ```
    改成
    ```
    ---
    layout: about
    title: Hi
    date: 2018-10-22
    comments: false
    type: about
    ---
    ```
    - markdown语法写about页面
    Hexo会把md文件自动生成html页面，用markdown语法去写md文件即可。

    - 重启server
    同样重启服务。
    ```
    hexo g && hexo s
    ```

5. #### 自定义页面

    在sidebar往往需要自定义一些自己需要的页面，这块我直接用我的留言板做例子。
    - new page
    在blog根目录，执行下列命令：
    ```
    hexo new page messagepad
    ```
    - 配置
    打开blog/source/messagepad/index.md，更改内容如下：
    ```
    ---
    title: 欢迎大家留言
    ---
    <img src="messagepad.png" style="border:3px solid blue;width:100%"/>
    
    <div class="ds-recent-visitors" data-num-items="28" data-avatar-size="42" id="ds-recent-visitors"></div>
    ```
    其实，在这个index.md文件中，就是用markdown语法去写html。
    - 配置 && 添加关键字
    其实这2步跟第三部分的**指定分类列表页**所做的操作一致。
    打开Next主题配置文件：blog/theme/next/_config.yml，搜索<font size="4" color="red">***menu***</font>，新增对应的sidebar栏：
    ```
    menu:
      messagepad: /messagepad/ || sticky-note
    ```
    打开文件：blog/themes/next/languages/zh-Hans.yml，添加对应的mapping：
    ```
    menu:
      messagepad: 留言板
    ```
    - 重启server
    同样重启服务。
    ```
    hexo g && hexo s
    ```

{% note info %}
### 技巧

打开blog/scaffolds/post.md文件，更改代码如下：
{% code %}
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
{% endcode %}
这样，在以后hexo new新建博文，模板自动生成，只需要添加对应的tags、categories即可
{% endnote%}
