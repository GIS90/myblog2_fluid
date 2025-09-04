# README

基于***Hexo*** + ***Fluid主题***搭建的静态博客网站，记录自己的工作、生活等内容，充实程序人生。

## 1、启动

- 本地Server
      hexo server -p 8888 -o

- 上传服务器

     需要在_config.yml配置文件中配置deploy，再用hexo g -d

## 2、版本信息

|   软件   | 版本     |    
| :----: | :----: | 
|  node    |  v23.11.1    |      
|   npm   |  10.9.2    |      
|   hexo   |    7.3.0   |     
|   hexo-cli   |    4.3.2   |     

## 3、配置

- 项目配置：_config.yml
- 主题配置：themes\fluid\_config.yml

## 4、部署

采用git部署到静态服务器，用git push进行云服务器仓库提交。

## 5、问题列表

1. 使用gi-endgi语法糖并排多个图片的时候，需要置顶image图片source绝对目录下的位置，不能用post自动建立文章的相对路径。
2. 最新版本的Hexo不支持cq-endcq语法糖。

## 6、个性化配置
用于配置个性化的文件，比如自定义css、博客首页、404页面等

### 6.1、自定义CSS
路径：source\custom\custom.css
### 6.2、公共文件
路径：source\public-files

## 7、自定义样式

直接基于Fluid主题源码进行样式修改，记录一下历史的修改。

### 7.1、文章详情底部版本新增左侧边框
文件位置：E:\github\myblog2_fluid\themes\fluid\source\css\_pages\_base\_widget\copyright.styl
样式类：.license-box
修改内容：
```css
-- 新增
border-left: 8px double rgb(255, 23, 0) !important;
```

### 7.2、修改归档（archives）页面提示词
文件位置：E:\github\myblog2_fluid\themes\fluid\layout\_partials\archive-list.ejs
修改内容：新增了2个span标签内容

```HTML
-- 新增
<span style="color: red;font-size: 42px;">你真帅!</span>
<span style="color: blue;font-size: 25px;">继续加油~~~</span>
```
### 7.3、工具推荐（links）页面的图标hover缩放大小调整
文件位置：E:\github\myblog2_fluid\themes\fluid\source\css\_pages\_links\links.styl
样式类：.link-avatar hover
修改内容：
```css
-- 修改
&:hover
      .link-avatar
        transform scale(1.3)
```

