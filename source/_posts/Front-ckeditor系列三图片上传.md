---
title: CkEditor图片上传到服务端
index_img: /img_index/index/20210130-001.jpg
categories:
    - [前端开发]
tags: [HTML, 前端组件, CkEditor, 富文本编辑器]
abbrlink: 32287
date: 2021-01-30 11:35:27
updated: 2021-01-30 11:35:27
desc: 富文本编辑器CkEditor的教程
keywords: CkEditor, 富文本, 编辑器, HTML
---


{% label primary@CKEditor系列教程（三） %} 




<!--more-->
<hr />

#### 效果

> 配置前

![](ckedit_1.png)

> 配置后

![](ckedit_2.png)

#### 图片上传配置

在config.js配置文件中进行图片上传的配置
```
//上传图片API
config.filebrowserImageUploadUrl = "/apis/uploadimg";
// 去掉图片预览框的文字
config.image_previewText = '';
// 隐藏“超链接”与“高级选项”只留上传和预览按钮
config.removeDialogTabs = 'image:advanced;image:Link;';
```

#### 后台代码

```
@apis.route('/uploadimg/', methods=['POST', 'OPTIONS'], strict_slashes=False)
def uploadimg():
    image = request.files.get('upload')
    no_res = jsonify({
            "uploaded": 0,
            "fileName": '',
            "url": ''
        })
    if not image:
        return no_res
    res = ApisService().store_to_imgae(image)
    json_res = json.loads(res)
    if json_res.get('status_id') != 100:
        return no_res

    return jsonify({
            "uploaded": 1,
            "fileName": json_res.get('data').get('name'),
            "url": json_res.get('data').get('url')
        })
```
上面是python对于图片存储的代码，store_to_imgae是实际存储的，需要的可以给我留言，要让前端ckedit能获取返回值，需要json格式，内容目标如下：
```
{
    "uploaded": 1,
    "fileName": json_res.get('data').get('name'),
    "url": json_res.get('data').get('url')
}
```
这样，前端就可以进行显示了。
