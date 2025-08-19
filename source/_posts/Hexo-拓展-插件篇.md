---
title: Hexo拓展-插件篇
index_img: /img_index/index/20190126-001.png
categories:
  - [Hexo]
tags: [Hexo, Hexo插件, Hexo美化]
top: false
desc: 关于Hexo搭建的博客拓展篇，关于博客用到的插件
keywords: hexo, next, Hexo, 美化, 插件, 博客, blog, plug, 插件, hexo-tag-echarts, hexo-admin, hexo-blog-encrypt
abbrlink: 36954
date: 2019-01-26 23:30:04
updated: 2019-07-26 23:30:04
---

<font color="red" size="5">***插件持续更新中～～～***</font>

<!-- more -->
<hr />

### 1、简介
{% note success %}
> 网站安装了许多插件，**hexo-symbols-count-time、hexo-tag-cloud、hexo-helper-live2d**等等，不止给站点增加了很多强大的功能以及给博文美化方面添加了不少样式、便利，我也一一记录下了常用的插件并写成文章发表。
> 但是去Hexo官方，看了一下插件，居然有290多个，也不能想Next主题内置的tags功能一样，每个都去尝试一下，起码把自己尝试的记录下来，与大家一起进行分享。
{% endnote %}




### 2、正文

#### 2.1、hexo-tag-echarts

> 作用

集成echart到博客中，用于数据展示，非常好用。

> 示例

![hexo-tag-echarts](hexo-tag-echarts.png)

> 代码

    ```
    {% echarts 400 '85%' %}
    {
        color: ['#F39C12'],
        tooltip : {
            trigger: 'axis',
            axisPointer : {            // 坐标轴指示器，坐标轴触发有效
                type : 'shadow'        // 默认为直线，可选为：'line' | 'shadow'
            }
        },
        grid: {
            left: '3%',
            right: '4%',
            bottom: '3%',
            containLabel: true
        },
        xAxis : [
            {
                type : 'category',
                data : ['Python','Linux','Hexo','LDAP','娱乐','影视说','旅行记','其他'],
                axisTick: {
                    alignWithLabel: true
                }
            }
        ],
        yAxis : [
            {
                type : 'value'
            }
        ],
        series : [
            {
                name:'直接访问',
                type:'bar',
                barWidth: '60%',
                data: [9, 2, 17, 2, 3, 2, 2, 5]
            }
        ]
    };
    {% endecharts %}
    ```


> <font size="4" color="red">**补充**</font>

hexo-tag-echarts安装 ：

{% code %}
npm install hexo-tag-echarts3 --save
{% endcode %}

hexo-tag-echarts说明：
```
1 || 展示的数据需要自己进行计算。
2 || echarts-endecharts 需要2个参数：容器高度、相对宽度。
3 || 这个插件的js有点问题，需要加些代码到文件：node_modules/hexo-tag-echarts/echarts-template.html，具体请查看官方手册。
```
hexo-tag-echarts资料 ：
```
hexo-tag-echarts3：https://github.com/kchen0x/hexo-tag-echarts3
echart：https://echarts.baidu.com/examples/
```

#### 2.2、 hexo-admin

> 作用

    hexo站点的后台管理。

> 示例

    ![hexo-admin](hexo-admin.png)

> 部署

- 安装插件
    ```
    npm install --save hexo-admin
    ```
- 启动服务
    ```
    hexo s
    ```
- 生成密码
    打开***http://127.0.0.1:8888/admin/***，找到**setting**菜单栏，找到***Setup authentification here***，点击->输入用户名、密码、密钥，在最下面有生成的配置信息。
- 配置
    打开blog/_config.yml文件，把上面生成的信息新增到配置文件最后。
    ```
    # admin后台user、password
    admin:
      username: pygo
      password_hash: $2a$10$h6ZFY9yMv.wIhd0aPQ/O4eB3k/zFiBj3aH0Zou8T9QK/H8e/Y/hUO
      secret: wuyananismywife
    ```

> <font size="4" color="red">**补充**</font>

 hexo-admin说明：
 ```
    1 || 切记是Hexo站点配置文件。
 ```
hexo-admin资料：
 ```
    https://github.com/jaredly/hexo-admin
 ```

 


#### 2.3、hexo-blog-encrypt

> 作用

    文章加密。

> 示例

    ![hexo-blog-encrypt](hexo-blog-encrypt.png)

> 部署

- 安装插件
    ```
    npm install --save hexo-blog-encrypt
    ```
- Hexo配置
    打开Hexo站点配置文件：blog/_config.yml，新增配置：
    ```
    # 文章加密功能
    encrypt:
        enable: true
    ```
- 文章配置
    打开一篇需要进行加密的.md文件，在顶部描述加入以下代码：
    ```
    password: 0803
    abstract: <div class="article_encrypt_abstract">暂不公开，请勿打扰 ～<font size="5" color="red">【状态：加密】</font></div>
    message: 密码提示：生日
    ```
        + password：博客密码
        + abstract：home主页显示的文字提示
        + message：查看博客，密码输入框上面的描述性文字
- 启动服务
    ```
    hexo s
    ```

> <font size="4" color="red">**补充**</font>

hexo-blog-encrypt说明：
```
    1 || 切记是Hexo站点配置文件。
```
 hexo-blog-encrypt资料：
 ```
    https://github.com/MikeCoder/hexo-blog-encrypt/blob/master/ReadMe.zh.md
 ```

> <font size="4" color="red">**自定义样式**</font>

关于加密的abstract && message样式比较难看，本人自定义了的样式，参考如下：
    {% code %}
    // 加密abstract样式
    .article_encrypt_abstract {
        text-align: center;
        color: #FFBE3D;
        font-size: 28px;
        -webkit-text-stroke: 1.2px #000000;
    }
    // 加密message样式
    .hbe-input-container label {
        color: deeppink!important;
        font-size: 32px!important;
        text-shadow: -1px 0 black, 0 1px black, 1px 0 black, 0 -1px black;
    }
    {% endcode %}

自定义样式文件：**blog/themes/next/source/css/_custom/custom.styl**

