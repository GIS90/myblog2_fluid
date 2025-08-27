---
title: Hexo菜单栏添加留言板-Valine
index_img: /img_index/index/20181208-001.png
desc: 在hexo博客菜单栏添加留言板功能，本博客使用的Valine插件
categories:
  - [Hexo]
tags: [Hexo, Hexo插件]
keywords: hexo, next, Hexo, 美化, 插件, 博客, blog, valine, 留言板, 菜单栏
abbrlink: 7953
date: 2018-12-08 20:44:08
updated: 2019-01-08 20:44:08
---

### 1、简述
{% note success %}
blog菜单栏添加留言板功能，欢迎大家留言交流。。。
{% endnote %}

<!--more-->
<hr />

***My blog***文章添加留言功能，除此之外还在菜单栏添加一个专门用来留言的message pad，用来***share***文章中简称mp。查看next主题config.yml文件以及google了一下，发现居然配置5中默认的评论系统。。。***next***这么变态吗？提供1～2就好了，5个是怎么回事啊，还得花时间去整理***compare***，以下仅代表个人观点。

### 2、评论系统

| id  |   type   | desc                                   | remark |
|:---:|:--------:|:-------------------------------------- |:------:|
|  1  |  valine  | https://valine.js.org                  | 🌟🌟🌟 |
|  4  |  disqus  | https://www.jianshu.com/p/c4f65ebe23ad |        |
|  5  | changyan | https://www.jianshu.com/p/5246d020da25 |        |
|  2  | gitment  | https://github.com/imsun/gitment       |   🌟   |
|  3  |  gitalk  | https://gitalk.github.io               |   🌟   |

#### 2.1、效果图
> valine

<img src="valine.png" style="border:3px solid black"/>
> disqus

<img src="disqus.png" style="border:3px solid black"/>
> changyan

<img src="changyan.png" style="border:3px solid black"/>
> gitment

<img src="gitment.png" style="border:3px solid black"/>
> gitalk

<img src="gitalk.png" style="border:3px solid black"/>

***总结：***<font color="#dd0000" size="4">单从效果图上来看，我倾向于gitment、gitalk、changyan，简单整洁。</font>

#### 2.2、功能性
这里不想说明了，直接给出结论。。。干货

***总结：***<font color="#dd0000" size="4">除了valine，其他4个评论均需要强制登录一个指定的账号才可以进行评论发布，令人太不爽了。</font>

### 3、Valine

    主要基于账号的问题，我选择Valine，起码大家都可以参与，没有限制。

#### 3.1、注册LeanCloud
注册LeanCloud，将数据托管给第三方，直接登录账号使用，很方便。注册地址：[注册Leancloud(官方)](https://leancloud.cn/)
> 注册

<img src="leancloud_zhuce.png" style="border:3px solid blue"/>
> 创建应用

<img src="leancloud_newapp.png" style="border:3px solid blue"/>
> 设置应用

<img src="leancloud_setting.png" style="border:3px solid blue"/>
> 应用key

<img src="leancloud_key.png" style="border:3px solid blue"/>


#### 3.2、next配置
打开next的配置：theme/next/_config.yml，搜索：# Valine
```
valine:
  enable: true # When enable is set to be true, leancloud_visitors is recommended to be closed for the re-initialization problem within different leancloud adk version.
  appid: XXXX # your leancloud application appid
  appkey: XXXX # your leancloud application appkey
  notify: false # mail notifier, See: https://github.com/xCss/Valine/wiki
  verify: false # Verification code
  placeholder: 欢迎吐槽 ！ # comment box placeholder
  avatar: mm # gravatar style
  guest_info: nick,mail # link custom comment header
  pageSize: 10 # pagination size
  language: # language, available values: en, zh-cn
  visitor: false # leancloud-counter-security is not supported for now. When visitor is set to be true, appid and appkey are recommended to be the same as leancloud_visitors' for counter compatibility. Article reading statistic https://valine.js.org/visitor.html
  comment_count: true # if false, comment count will only be displayed in post page, not in home page
```
- enable：是否开启Valine评论
- appid：leancloud应用上App ID
- appkey：leancloud应用上App Key
- notify && verify：是否开启邮件提醒，leancloud && github valine使用有详细介绍，具体查看文档(https://valine.js.org/notify.html)
- placeholder：评论区默认文字
- avatar：头像设置(https://valine.js.org/avatar.html)
- guest_info：评论区用户选填的基本信息
- pageSize：一页评论的数据
- language：语言设置，默认zh-cn
- visitor：还不知道，欢迎大家留言告诉我，哈ヾﾉ≧∀≦)o哈
- comment_count：是否展示评论总数

### 4、评论生效
***注册 && 配置***搞完了之后，并不是立马看到效果，需要：

    hexo g
    hexo server -p 8888

从心打开blog，奇迹出现了。。。哈哈哈，***<font color="#dd0000">一起都是自己的功劳，并不是什么奇迹，只要努力，成功离你就会不远</font>***。

### 5、禁用评论
但是，我其实就是想在菜单栏添加个留言板功能，不想再blog文章中添加这个功能，简单。打开文章，在最上面的设置中加入

    comments: true
从新g->server，奇迹再次发生。。。如果想都关闭评论，这个需要在已经发布的文章中分别设置，暂时没有找到简单的方法。不能在以后每篇文章中每次都设置，好麻烦，只想需要要设置一次。当然有方法，blog/scaffolds/post.md文件设置：
```
---
title: {{ title }}
desc:
date: {{ date }}
updated: {{ date }}
categories:
tags:
comments: false
---
```

### 6、留言板
***终于进入主题了，菜单栏添加单独一栏留言板功能。***
> 添加page

    hexo new page messagepad

> 留言板

对page messagepad的index.md进行编辑加入自定义的东西。
```
---
title: 欢迎大家留言
---
![messagepad.png](messagepad.png)
<div class="ds-recent-visitors" data-num-items="28" data-avatar-size="42" id="ds-recent-visitors"></div>
```
继续见证奇迹吧。

### 7、Suggestion
***<font color="#dd0000" size="5">真正的成功并非一朝一夕，水滴石穿，努力吧，少年们！！！</font>***

    到此收工，一个完美的菜单栏留言板功能share献给大家。
