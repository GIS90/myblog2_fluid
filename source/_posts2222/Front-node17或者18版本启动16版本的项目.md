---
title: 'NODE17或者18版本启动NODE16版本的项目:0308010C:digital envelope routines::unsupported'
index_img: /img_index/index/20230218-001.png
categories:
  - 前端开发
tags:
  - VUE
  - NPM
  - NODE
desc: 'NODE17或者18版本启动NODE16版本的项目:0308010C:digital envelope routines::unsupported'
keywords: 'VUE, NPM, NODE, 0308010C'
abbrlink: 40755
date: 2023-02-18 19:46:20
updated: 2023-02-18 19:46:20
---

{% note primary %}
#### 问题背景
前端项目open2lui一直都用的是node/16.9.1版本，最近用n更新了最新版、lts版本，分别是node/18.14.0、node/19.6.0，完了用18活着19版本npm去运行npm run dev的时候报错了，导致项目起不来，后台查询资料解决了高版本启动项目的问题。
{% endnote %}


{% label primary@NODE %} {% label warning@VUE %}

<!--more-->
<hr />


在项目的package.json文件中记录了npm scripts，如下：
```
"scripts": {
    "dev": "vue-cli-service serve --open --copy --mode development",
    "devs": "vue-cli-service serve --open --copy --https --mode development",
    "prod": "vue-cli-service serve --open --copy --mode production",
    "prods": "vue-cli-service serve --open --copy --https --mode production",
    "lint": "eslint --ext .js,.vue src",
    "lint-fix": "eslint --ext .js,.vue src --fix",
    "build": "vue-cli-service build --report",
    "build:prod": "vue-cli-service build --report --mode production --dest dist",
    "build:dev": "vue-cli-service build --report --mode development --dest dist",
    "preview": "node build/index.js --preview",
    "new": "plop",
    "svgo": "svgo -f src/icons/svg --config=src/icons/svgo.yml",
    "test:unit": "jest --clearCache && vue-cli-service test:unit",
    "test:ci": "npm run lint && npm run test:unit"
},
```
用18/19高版本的node运行***npm run dev***，发生了错误。

#### 错误

```
Error: error:0308010C:digital envelope routines::unsupported
    at new Hash (node:internal/crypto/hash:71:19)
    at Object.createHash (node:crypto:133:10)
    at module.exports (/Users/gaomingliang/gitee/open2lui/node_modules/webpack/lib/util/createHash.js:135:53)
    at NormalModule._initBuildHash (/Users/gaomingliang/gitee/open2lui/node_modules/webpack/lib/NormalModule.js:417:16)
    at handleParseError (/Users/gaomingliang/gitee/open2lui/node_modules/webpack/lib/NormalModule.js:471:10)
    at /Users/gaomingliang/gitee/open2lui/node_modules/webpack/lib/NormalModule.js:503:5
    at /Users/gaomingliang/gitee/open2lui/node_modules/webpack/lib/NormalModule.js:358:12
    at /Users/gaomingliang/gitee/open2lui/node_modules/loader-runner/lib/LoaderRunner.js:373:3
    at iterateNormalLoaders (/Users/gaomingliang/gitee/open2lui/node_modules/loader-runner/lib/LoaderRunner.js:214:10)
    at Array.<anonymous> (/Users/gaomingliang/gitee/open2lui/node_modules/loader-runner/lib/LoaderRunner.js:205:4)
    at Storage.finished (/Users/gaomingliang/gitee/open2lui/node_modules/enhanced-resolve/lib/CachedInputFileSystem.js:55:16)
    at /Users/gaomingliang/gitee/open2lui/node_modules/enhanced-resolve/lib/CachedInputFileSystem.js:91:9
    at /Users/gaomingliang/gitee/open2lui/node_modules/graceful-fs/graceful-fs.js:123:16
    at FSReqCallback.readFileAfterClose [as oncomplete] (node:internal/fs/read_file_context:68:3) {
  opensslErrorStack: [ 'error:03000086:digital envelope routines::initialization error' ],
  library: 'digital envelope routines',
  reason: 'unsupported',
  code: 'ERR_OSSL_EVP_UNSUPPORTED'
}

Node.js v18.14.0
npm notice
npm notice New minor version of npm available! 9.3.1 -> 9.4.2
npm notice Changelog: https://github.com/npm/cli/releases/tag/v9.4.2
npm notice Run npm install -g npm@9.4.2 to update!
npm notice
```


#### 问题原因

对前端不是特别精通，具体什么原因上网查了下，大致理解原因：
出现这个错误是因为node.js V17、18、19高版本中最近发布的OpenSSL3.0, 而OpenSSL3.0对允许算法和密钥大小增加了严格的限制，可能会对生态系统造成一些影响.

#### 解决方案

> 方案一

在控制台中进行变量设置：
```
export NODE_OPTIONS=--openssl-legacy-provider
```
把NODE_OPTIONS变量设置为环境变量，在运行的时候直接export导入，***npm run dev***运行。

> 方案二

直接修改package.json scripts内容如下：
```
export NODE_OPTIONS=--openssl-legacy-provider && vue-cli-service serve --open --copy --mode development
```
运行***npm run dev***，高版本node的问题解决。
但是，这么处理有个问题，就是其他的script脚本也需要加上变量，不如直接在控制台活着配置文件中添加。

#### 开发工具

IDE：WebStorm
OS：MacOS
node多版本管理用的为n。


<font size=6.5 color='red'>坚持学习。。。。。。</font>
