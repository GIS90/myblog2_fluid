---
title: Next主题优化-内置tags篇
index_img: /img_index/index/20181226-001.png
desc: Next主题自带tags-博文美化篇（note、tabs、cq、button、label、exturl、fullimage、grouppicture、include_raw、pdf、mermaid）
categories:
  - [Hexo]
tags: [Hexo, Hexo美化]
keywords: hexo, next, Hexo, 美化, 插件, 博客, blog, note, tabs, cq, button, label, exturl, fullimage, grouppicture, include_raw, pdf, mermaid
abbrlink: 25933
date: 2018-12-26 22:15:34
updated: 2018-12-26 22:15:34
---

{% label primary@Hexo美化 %} {% label primary@Gemini %} {% label info@Next主题优化 %}

<font color="#dd0000" size="4">目标：</font><font color="blue" size="6">美化博文</font>

### 背景
{% note success %}
发布博客有段时间了，而且陆陆续续写了一些文章，但页面样式都比较简单，只是单一的MarkDown语法，难道是***Hexo + Next + MarkDown***只能写出简单样式的文章？有点不相信，于是去[Next官方](http://theme-next.iissnan.com/)。看了一些使用文档，查看了***<font color="#dd0000" size="4">内建标签</font>***，原来Next可以让简单样式的文章可以变得那么好看，总结了一下分享出来。
{% endnote %}

<!--more-->

<hr />

### 前言
Next主题使用的***<font color="#dd0000" size="5">内置tag</font>***，官方都已经写好js，直接使用对应的语法糖加入到文章中，就可以实现多样式、多功能的效果。下面分别说明了一下本人博客Hexo+Next的版本，如果有加入对应tag没有效果的，可以留言给我，一起交流一起学习。

<font color="#dd0000" size="4">Next主题内置tag样式都是本人经过实践得出，原创之作，欢迎大家进行转载。</font>

### 版本介绍
| id  |     name     | version |  remark  |
|:---:|:------------:|:-------:|:--------:|
|  1  |     Hexo     | v3.8.0  | 系统版本 |
|  2  | NextT.Pisces | v7.0.1  | 主题版本 |

***<font color="#dd0000" size="4">内置tag文件位置：blog/themes/next/scripts/tags。</font>***

### Tags

内置tags的js文件一共有12个，在这里我只讲解本人实践的tag。

1. #### tabs


> 功能

提供了一个tabs页样式标签，可以进行切换。

> 代码

```
{% tabs t_code_1 %}
<!-- tab 标题一 -->
tab1
<!-- endtab -->
<!-- tab 标题二 -->
tab2
<!-- endtab -->
<!-- tab 标题三 -->
tab3
<!-- endtab -->
{% endtabs %}
```

> 配置

打开Next配置文件：blog/theme/next/_config.yml，搜索<font size="4" color="red">***tabs***</font>，更改代码如下：
```
# tabs相关配置
tabs:
  enable: true
  transition:
    tabs: false
    labels: true
  border_radius: 5  # tab圆角设置
```

> 示例

```
{% tabs t1 %}
<!-- tab 标题一 -->
tab1
<!-- endtab -->
<!-- tab 标题二 -->
tab2
<!-- endtab -->
<!-- tab 标题三 -->
tab3
<!-- endtab -->
{% endtabs %}
```

> 语法糖

- tabs-endtabs 是***tab tag***语法糖。
- tab-endtab 是tabs里一个完整的tab，多个tab只需要更换标题、内容即可。

2. #### cq && centerquote


> 功能

文本居中、引用样式。

> 代码

```
{% cq %}
一个爱老婆的Python程序猿。。。。。。
**PyGo²**
{% endcq %}
```

> 语法糖

- cq-endcq 是***cq tag***语法糖。
- cq与centerquote等价。


3. #### note


> 功能

**内置note**标签。

> 代码

```
{% note default %}
default 提示块标签
{% endnote %}

{% note primary %}
primary 提示块标签
{% endnote %}

{% note success %}
success 提示块标签
{% endnote %}

{% note info %}
info 提示块标签
{% endnote %}

{% note warning %}
warning 提示块标签
{% endnote %}

{% note danger %}
danger 提示块标签
{% endnote %}
```

> 配置

打开Next配置文件：blog/theme/next/_config.yml，搜索<font size="4" color="red">***Note tag***</font>，更改代码如下：
```
# note tag 相关配置
note:
  style: flat
  icons: true   # 是否启用图标
  border_radius: 3  #圆角设置
  light_bg_offset: 0     设置0为里面内容为note样式包含
```

> 示例

{% note default %}
default
提示块标签
{% endnote %}

{% note primary %}
primary
提示块标签
{% endnote %}

> 语法糖

- note-endnote 是***cq tag***语法糖。
- 样式：default、primary、success、info、warning、danger，示例中我只展示了2种。


4. #### label


> 功能

**内置label**标签。
    
> 代码

```
{% label default@default %}
{% label primary@primary %}
{% label success@success %}
{% label info@info %}
{% label warning@warning %}
{% label danger@danger %}
```

> 示例

{% label default@default %} {% label primary@primary %} {% label success@success %}
<HR />
{% label info@info %} {% label warning@warning %} {% label danger@danger %}

<HR />

> 语法糖

```
- label 是***label tag***语法糖，不需要加endlabel标识结束。
- @符号前面是label样式。
- @符号后面是label内容。
- 说明label需要2个参数，有兴趣的可以查看：blog/themes/next/scripts/tags/label.js

args = args.join(' ').split('@');
var classes = args[0] || 'default';
var text    = args[1] || '';
```

5. #### button && btn


> 功能

**内置button**标签。
    
> 代码

```
{% btn https://www.baidu.com, 百度首页, download fa-lg fa-fw %}
```

> 示例
```
{% btn https://www.baidu.com, 百度首页, download fa-lg fa-fw, 百度首页标题 %}
```

> 语法糖

- btn 是***btn tag***语法糖，不需要加endbtn标识结束。
- 这个标签需要4个参数: url地址、button文本内容、button icon、button title。
- 第四个参数我试了，没什么效果，有了解的小伙伴欢迎留言。
- 这个按钮的样式跟Next主题样式一致，因为按钮样式本人优化过，所以在示例中的button与Next默认样式不一致。
- 说明button需要4个参数，有兴趣的可以查看：blog/themes/next/scripts/tags/button.js
```
args = args.join(' ').split(',');
var url   = args[0];
var text  = args[1] || '';
var icon  = args[2] || '';
var title = args[3] || '';
```
- btn与button等价。


6. #### ~~exturl && extlink(过时，不建议使用)~~


> 功能

外链街功能。
    
> 代码

```
{% exturl https://www.baidu.com 百度首页 %}
```

> 示例
```
{% exturl https://www.baidu.com 百度首页 %}
```

> 语法糖

- exturl 是***exturl tag***语法糖，不需要加endexturl标识结束。
- 看js文件，这个标签需要5个参数: url地址、url文本内容、urltitle、url item、0、结束位置，其中url文本内容是个列表。
- 说明button需要4个参数，有兴趣的可以查看：blog/themes/next/scripts/tags/exturl.js
```
var exturl = 'exturl';
var url    = '';
var text   = [];
var title  = '';
var item   = '';
var i      = 0;
var len    = args.length;
```
- exturl与extlink等价。
{% note info %}
特殊说明
{% endnote %}
在md文档中写完此标签，怎么重启，刷新，发现外链都不管用，研究了好一会儿，我在这里就不买官司了，直接看解决方法：
- 打开Next主题的配置文件，搜索***exturl***
- 更改代码如下
```
exturl: true
```
- 重新启动服务 && 刷新。




7. #### fi && fullimage


> 功能

全像显示图片。

> 代码

```
{% fi /images/article_wdyxxy.jpg %}
<img src="next_youhua_avatar.gif" style="border:1.5px solid blue"/>
```

> 示例
```
{% fi /images/article_wdyxxy.jpg %}
```

> 语法糖

- fi 是***fullimage tag***语法糖，无对应的end结束。
- fi与fullimagee等价。
- 我试了几个发现这个***fi tag***与正常***imag**标签没什么区别，上面示例代码中给出了，效果图是一样的。



8. #### gp && grouppicture


> 功能

多格局显示图片。

{% note info %}
这里不做简述，请查看博文：hexo博文展示并排等多样式图片
{% endnote %}

9. #### include_raw


> 功能

引用html文件，是把部分html代码单独写在一个html文件里面，通过**tag**标签把文件引入的语法，需要可以看最下面的官方说明。

10. #### pdf


> 功能

通过**pdf tag**标签把pdf文件引入页面中。
    
> 代码

```
{% pdf /images/PythonStudy.pdf %}
```

> 配置

- 安装theme-next-pdf
```
cd blog/themes/next
git clone https://github.com/theme-next/theme-next-pdf source/lib/pdf
```

打开Next配置文件：blog/theme/next/_config.yml，搜索<font size="4" color="red">pdf</font>，更改代码如下：
```
# pdf相关配置
# See: https://github.com/theme-next/theme-next-pdf
pdf:
  enable: true
  height: 500px  # 设置默认高度
  pdfobject:
    cdn: //cdn.jsdelivr.net/npm/pdfobject@2/pdfobject.min.js
```
- 新建目录，如果想要把文件单独存放一个目录，那么需要在blog/public目录下新建目录；如果不想新建，可以把文件放在blog/public/images目录下，这个目录是单独用来存放站点图片之用的目录。
    - 在***pdf tag***，参数的路径写上上面新建的。
    
> 示例

    {% pdf /publicfiles/PythonStudy.pdf %}

> 语法糖

- pdf 是***pdftag***语法糖，无对应的end结束。
  

{% note info %}
建议：
    建议少用此标签，因为页面如果加载大文件会很慢。
{% endnote %}



11. #### mermaid


> 功能

用了好几个翻译都解释“美人鱼”，但是实现的效果类似于word、excel、ppt里面的架构图，通过**mermaid tag**标签把架构图引入页面中。
    
> 代码

```
{% mermaid graph TD %}
A[Christmas] -->|Get money| B(Go shopping)
B --> C{Let me thinksssss<br/>ssssssssssssssssssssss}
C -->|One| D[Laptop]
C -->|Two| E[iPhone]
C -->|Three| F[Car]
{% endmermaid %}
```

> 配置

- 打开Next配置文件：blog/theme/next/_config.yml，搜索<font size="4" color="red">***mermaid***</font>，更改代码如下：
```
# Mermaid tag
mermaid:
  enable: true
  # Available themes: default | dark | forest | neutral
  theme: forest   # 主题
  cdn: //cdn.jsdelivr.net/npm/mermaid@8/dist/mermaid.min.js
```
关于架构图的样式取决于theme参数，每个样式我都试了一下，还是forest最漂亮。
    
> 示例

```
{% mermaid graph TD %}
A[Christmas] -->|Get money| B(Go shopping)
B --> C{Let me thinksssss<br/>ssssssssssssssssssssss}
C -->|One| D[Laptop]
C -->|Two| E[iPhone]
C -->|Three| F[Car]
{% endmermaid %}
```

> 语法糖

- mermaid-endmermaid 是***mermaid tag***语法糖。
- 例子我就采用官方的例子了，没自己去写，官方有好几种写法、样式，需要的请查阅官方资料。



12. #### video


> 功能

html对视频的引用。
    
> 代码

```
{% video url %}
{% video /publicfiles/xiao6.mp4 %}
```

> 配置

- 我在blog/public目录下新建了publicfiles一个目录用来存放上面**pdf&&video**用到的文件。

> 示例

```
{% video /publicfiles/xiao6.mp4 %}
```


> 语法糖

- video 是***video tag***语法糖，无endvideo结束。
- 如果是引用视频，找到视频资源，直接写上url地址即可。

{% note success %}
已解决问题
{% endnote %}

开启服务运用的是debug模式，方便进行调试，在启用的时候，发现服务报了好多***WARNING***，总结下来方便大家解决问题。

> `exturl` and `extlink` tag will not longer be supported.

翻译出来就是将不再支持`exturl`和'extlink`的标记，在我的blog很少用到，用到的会用***button tag***进行代替。

> Tabs block must have unique name!

找到tabs-endtabs内置标签，在tabs后面加上tab一个名字，而且要***unique***。

{% note warning %}
未解决问题
{% endnote %}

在文章编写过程中遇到了一些问题，先记录下来，以后解决在来修改。
- 在tab里面放\`\`\`代码块\`\`\`，不生效。
- button与exturl共存在同一页面，并启动exturl配置，发现button样式没了，可能是我自己瞎改button造成的原因，问题解决中。

### 学习

Next中文官方：http://theme-next.iissnan.com/tag-plugins.html
Next Tag官方：https://hexo.io/docs/tag-plugins.html
botton：https://theme-next.org/docs/tag-plugins/button/
note：https://theme-next.org/docs/tag-plugins/note/
tabs：https://theme-next.org/docs/tag-plugins/tabs/
cq：https://theme-next.org/docs/tag-plugins/
label：https://theme-next.org/docs/tag-plugins/label/
exturl：https://theme-next.org/docs/tag-plugins/exturl/
fullimage：https://theme-next.org/docs/tag-plugins/full-image
group-pictures：https://theme-next.org/docs/tag-plugins/group-pictures
pdf tag：https://theme-next.org/docs/tag-plugins/pdf
theme-next-pdf：https://github.com/theme-next/theme-next-pdf
mermaid：https://theme-next.org/docs/tag-plugins/mermaid/
video：https://theme-next.org/docs/tag-plugins/video


### 结束语

到这里，Next内置的tag都实践完毕，根据需要使用上述tags，给博文带来了不一样的视觉效果以及便利，欢迎大家转载，一起交流使用。

***<font color="blue" size="5">最好的开始就是从现在开始！！！</font>***
