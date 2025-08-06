# README

基于***Hexo*** + ***Fluid主题***搭建的静态博客网站，记录自己的工作、生活等内容，充实程序人生。

## 1、启动

- 本地Server
      hexo server -p 8888 -o

- 上传服务器

     需要在_config.yml配置文件中配置deploy，再用hexo g -d

## 2、配置

- 项目配置：_config.yml
- 主题配置：themes\fluid\_config.yml

## 3、部署

采用git部署到静态服务器，用git push进行云服务器仓库提交。

## 4、问题列表

1、使用gi-endgi语法糖并排多个图片的时候，需要置顶image图片source绝对目录下的位置，不能用post自动建立文章的相对路径。

## 5、自定义样式

直接基于Fluid主题源码进行样式修改，记录一下历史的修改。

### 1、文章版本新增左侧边框

文件位置：E:\github\myblog2_fluid\themes\fluid\source\css\_pages\_base\_widget\copyright.styl

样式类：license-box

修改内容：

```css
-- 新增
border-left: 8px double rgb(255, 23, 0) !important;
```

