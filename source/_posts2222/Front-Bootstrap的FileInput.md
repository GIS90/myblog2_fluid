---
title: Bootstrap之FileInput的基础学习
index_img: /img_index/index/20210619-001.jpg
categories:
  - [前端开发]
tags: [Bootstrap]
abbrlink: 24956
date: 2021-06-19 23:45:35
updated: 2021-06-19 23:45:35
desc: Bootstrap的FileInput基础学习
keywords: Bootstrap, FileInput, File, Input
---


{% note primary %}
最近在做一个上传文件的功能，自己写的样式那叫一个难看啊。。。。。上网找了一下，发现有Bootstrap封装好的、而且重要的是还免费的FileInput，拿来用就可以了。
{% endnote %}

{% label primary@Bootstrap %} {% label info@FileInput %}

<!--more-->
<hr />

#### 示例

官网：https://plugins.krajee.com/file-basic-usage-demo
中文：http://www.bootstrap-fileinput.com/examples.html

#### 安装

安装有很多种方式，看<a href="http://www.bootstrap-fileinput.com/" target="_blank" class="block_project_a">中文官网</a>。
我直接github上下载的源码文件，直接引用，我使用的是python+flask框架的模板。
```
<script type="text/javascript" src="{{ url_for('static', filename='plugins/bootstrap-fileinput/js/fileinput.min.js') }}"></script>
<link rel="stylesheet" href="{{ url_for('static', filename='plugins/bootstrap-fileinput/css/fileinput.min.css') }}">
<script type="text/javascript" src="{{ url_for('static', filename='plugins/bootstrap-fileinput/themes/fas/theme.min.js') }}"></script>
<script type="text/javascript" src="{{ url_for('static', filename='plugins/bootstrap-fileinput/themes/explorer-fas/theme.min.js') }}"></script>
<link rel="stylesheet" href="{{ url_for('static', filename='plugins/bootstrap-fileinput/themes/explorer-fas/theme.min.css') }}">
<script type="text/javascript" src="{{ url_for('static', filename='plugins/bootstrap-fileinput/js/locales/zh.js') }}"></script>
```

#### 使用

> HTML代码

我才用了modal+fileinput的方式使用。
```
<div class="modal-body">
    <input id="fileinput_1" type="file">
</div>
```

> JS代码

```
$(function(){
    $('[data-toggle="tooltip"]').tooltip(); // 也可以使用 $('#text').tooltip();

    // bootstrap fileinput: https://github.com/kartik-v/bootstrap-fileinput
    /* init default, config: http://www.bootstrap-fileinput.com/options.html
    showUpload: true, //是否显示上传按钮
    showCaption: true, //是否显示标题
    showPreview: true, //是否显示文件预览
    showRemove: true, //是否显示删除/清除按钮
    showCancel: true, //是否显示文件上传取消按钮
    showClose: true, //是否显示预览界面的关闭图标
    showUploadedThumbs: true, //否在预览窗口中持续显示已经上传的文件缩略图
    showBrowse: true, //是否显示文件浏览按钮
    dropZoneEnabled: true, //是否显示拖拽区域
    browseOnZoneClick: true, //是否在点击预览区域时触发文件浏览/选择
    uploadClass: "btn btn-primary", //上传按钮样式
    removeClass: "btn btn-danger", //移除按钮样式
    maxImageWidth: 1000,//图片的最大宽度
    maxImageHeight: 1000,//图片的最大高度
    minImageWidth: 30, //图片的最小宽度
    minImageHeight: 30, //图片的最小高度
    overwriteInitial: false, // 是否要覆盖初始预览内容和标题设置
    previewFileIconSettings: {
        'doc': '<i class="fa fa-file-word-o text-primary"></i>',
        'xls': '<i class="fa fa-file-excel-o text-success"></i>',
        'ppt': '<i class="fa fa-file-powerpoint-o text-danger"></i>',
        'jpg': '<i class="fa fa-file-photo-o text-warning"></i>',
        'pdf': '<i class="fa fa-file-pdf-o text-danger"></i>',
        'zip': '<i class="fa fa-file-archive-o text-muted"></i>',
    }
    uploadAsync: true,  //bool whether the batch upload of multiple files will be asynchronous/in parallel. Defaults to true.
    */
    // theme: bs5 fa fas gly explorer explorer-fa explorer-fas
    $("#fileinput_1").fileinput({
        theme: 'fas',
        language: 'zh', //设置语言
        uploadUrl: '/common/upload_file/',  //上传的地址
        uploadAsync: false,
        allowedFileExtensions: ['pdf'],  //允许的文件拓展类型
        browseOnZoneClick: true, //是否在点击预览区域时触发文件浏览/选择
        frameClass: 'krajee-default',
        browseClass: "btn btn-primary", //按钮样式
        maxFileSize: 0,//单位为kb，如果为0表示不限制文件大小
        minFileCount: 0,
        maxFileCount: 1, //表示允许同时上传的最大文件个数
        enctype: 'multipart/form-data',
        uploadExtraData:function(){
            return {'transfer': '1'};
        }
    });

    $("#fileinput_m").fileinput({
        theme: 'explorer-fas',
        language: 'zh', //设置语言
        uploadUrl: '/common/upload_file/',  //上传的地址
        uploadAsync: false,
        allowedFileExtensions: ['pdf'],  //允许的文件拓展类型
        browseOnZoneClick: true, //是否在点击预览区域时触发文件浏览/选择
        frameClass: 'krajee-default',
        browseClass: "btn btn-primary", //按钮样式
        maxFileSize: 0,//单位为kb，如果为0表示不限制文件大小
        minFileCount: 0,
        enctype: 'multipart/form-data',
        //maxFileCount: 5, //表示允许同时上传的最大文件个数
        hideThumbnailContent: true,
        uploadExtraData:function(){
            return {'transfer': '1'};
        }
    });
});

// 单文件上传监听
$('#fileinput_1').on('fileuploaded', function(event, data, previewId, index) {
    var status_id = data.response.status_id;
    if (status_id == 100){
        toastr.success(data.response.msg, "温馨提示：");
    }else if(200 < status_id && status_id < 500){
        toastr.warning(data.response.msg, "温馨提示：");
    }else{
        toastr.error(data.response.msg, "温馨提示：");
    }
});

$('#fileinput_1').on('filebatchuploadsuccess', function(event, data, previewId, index) {
    var status_id = data.response.status_id;
    if (status_id == 100){
        toastr.success(data.response.msg, "温馨提示：");
    }else if(200 < status_id && status_id < 500){
        toastr.warning(data.response.msg, "温馨提示：");
    }else{
        toastr.error(data.response.msg, "温馨提示：");
    }
});

// 多文件上传监听
$('#fileinput_m').on('fileuploaded', function(event, data, previewId, index) {
    var status_id = data.response.status_id;
    if (status_id == 100){
        toastr.success(data.response.msg, "温馨提示：");
    }else if(200 < status_id && status_id < 500){
        toastr.warning(data.response.msg, "温馨提示：");
    }else{
        toastr.error(data.response.msg, "温馨提示：");
    }
});

$('#fileinput_m').on('filebatchuploadsuccess', function(event, data, previewId, index) {
    var status_id = data.response.status_id;
    if (status_id == 100){
        toastr.success(data.response.msg, "温馨提示：");
    }else if(200 < status_id && status_id < 500){
        toastr.warning(data.response.msg, "温馨提示：");
    }else{
        toastr.error(data.response.msg, "温馨提示：");
    }
});

// 监听fileinput modal关闭重新拉取数据
$('#fileInputModal').on('hidden.bs.modal', function () {
    draw_table();
});

$('#manyFileInputModal').on('hidden.bs.modal', function () {
    draw_table();
});
```

> 后台python

```
view:

@common.route('/upload_file/', methods=['GET', 'POST'], strict_slashes=False)
def upload_file():
    if request.method == 'GET':
        return Status(
            201,
            'failure',
            u'upload_file API请求方法错误',
            {}
        ).json()

    try:
        files = request.files.getlist('file_data')
        res = CommonService().upload_file(files,
                                          transfer=request.values.get('transfer'),
                                          is_start_fmt=False)
#################################################################################################################################################
# controller:

def upload_file(self, files, transfer, is_start_fmt=True):
    if not files:
        return Status(
            206,
            'failure',
            u'没有发现上传文件',
            {}
        ).json()

    # check office_transfer
    office_transfer_models = self.enums_bo.get_all_by_name(name='office_transfer')
    office_transfers = list()
    for ot in office_transfer_models:
        if not ot or not ot.sub_name: continue
        office_transfers.append(ot.sub_name)
    if transfer not in office_transfers:
        return Status(
            203,
            'failure',
            u'transfer值参数不合法',
            {}
        ).json()

    success_list = list()
    failure_list = list()
    for f in files:
        if not f: continue
        f_name = f.filename
        if is_start_fmt and not self.file_lib.allow_format_fmt(f_name):
            failure_list.append(f_name)
            continue
        is_ok, store_msg = self.file_lib.store_file(f, compress=False, is_md5_store_name=False)
        if not is_ok:
            failure_list.append(f_name)
            continue
        store_msg['name'] = f_name
        is_to_db = self.__store_file_to_db(store_msg, transfer)
        success_list.append(f_name) if is_to_db else failure_list.append(f_name)
    if files and len(failure_list) == len(files):
        return Status(
            210,
            'failure',
            u'文件上传失败',
            {}
        ).json()
    if failure_list:
        return Status(
            303,
            'failure',
            u'文件上传成功：%s，失败：%s' % (len(success_list), len(failure_list)),
            {}
        ).json()
    return Status(
        100,
        'success',
        u'文件上传成功',
        {}
    ).json()

#################################################################################################################################################
# services:

def store_file(self, file, compress=False, is_md5_store_name=True):
    if not file:
        return False, {'message': '没有发现文件'}
    try:
        # 文件存储初始化
        now_date = get_now(format="%Y-%m-%d")
        real_store_dir = get_base_dir() + TRANSFER_BASE_DIR + now_date
        store_dir = os.path.join(TRANSFER_BASE_DIR + now_date)
        if not os.path.exists(real_store_dir):
            mk_dirs(real_store_dir)

        file_name = file.filename
        md5_v, store_file_name = get_storefilename_by_md5(file_name, ftype='file')
        _real_file = os.path.join(real_store_dir, store_file_name) \
            if is_md5_store_name else os.path.join(real_store_dir, file_name)
        file.save(_real_file)
        # 压缩
        # if compress:
        #     pass
        return True, {'store_name': store_file_name if is_md5_store_name else file_name,
                      'md5': md5_v,
                      'path': os.path.join(store_dir, store_file_name) if is_md5_store_name else os.path.join(store_dir, file_name),
                      'message': 'success'}
    except:
        return False, {'message': '文件存储发生错误'}

```

#### 参考学习

- 中文官网：http://www.bootstrap-fileinput.com/
- bootstrap-fileinput源码：https://github.com/kartik-v/bootstrap-fileinput
- bootstrap-fileinput在线API：http://plugins.krajee.com/file-input
- bootstrap-fileinput Demo展示：http://plugins.krajee.com/file-basic-usage-demo
