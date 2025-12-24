---
title: 关于FastAPI Swagger-UI本地js文件配置
index_img: /img_index/index/2025-12-23-001.png
tags: [Python]
categories: [Python]
abbrlink: 1877
date: 2025-12-18 21:50:18
updated: 2025-12-18 21:50:18
---



### 背景

最近在写关于Fastsolt-API脚手架，基于FastAPI Web框架，在启动完APP后通过dosc地址查看所有API以及对应的模型，还可以进行接口测试，是非常不错的，但是没有网络或者网络不好的时候发现Swagger UI界面出不来，查询才发现，界面会加载3个静态的js文件导致。

<!--more-->
<hr />



### 配置

1、下载Swagger-UI文件：从 swagger-ui-dist 下载最新版本，将dist文件夹内的文件（如swagger-ui-bundle.js）放入项目的static目录下。
2、挂载静态文件目录：在主应用文件中，挂载static目录。
```
python
from fastapi import FastAPI
from starlette.staticfiles import StaticFiles


# FastAPI App instance
app: FastAPI = FastAPI()
# 挂载静态目录
app.mount("/static", StaticFiles(directory="static"), name="static")

@app.get(app_docs_url, include_in_schema=False)
    async def __swagger_ui_html():
        return get_swagger_ui_html(
            openapi_url=app_openapi_url,
            title=f"{server_name}-{server_version}-接口说明手册",
            swagger_js_url=f"{app_static_url}/swagger-ui-bundle.js",
            swagger_css_url=f"{app_static_url}/swagger-ui.css",
            swagger_favicon_url=f"{app_static_url}/favicon.ico"
        )
```
3、再次访问docs地址的时候就会加载本地js文件了。
