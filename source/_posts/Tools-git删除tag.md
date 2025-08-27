---
title: GIT删除tag
index_img: /img_index/index/20211215-001.png
categories:
  - 工具集
tags:
  - git
desc: 总结一下GIT删除本地tag以及远程tag的操作
keywords: 'git, 服务器, 版本管理, 代码发布, linux'
abbrlink: 53395
date: 2021-12-15 20:15:25
updated: 2021-12-15 20:15:25
---


{% note info %}
<font color='red' size=4.5>总结一下GIT删除本地tag以及远程tag的操作！！！。</font>
{% endnote %}




<!--more-->
<hr />

示例中以v-2021-12-15-01为tag示例。




#### 删除本地tag

```
git tag -d v-2021-12-15-01
```

![](loca_del.png)

#### 删除远程tag

```
git push origin :refs/tags/v-2021-12-15-01
```

![](remote_show.png)

#### 显示本地tag

```
git tag -l
```

![](local_show.png)

#### 显示远程tag

```
git show-ref --tag
```

![](remote_del.png)

#### 删除多个

```
# 本地
git tag -l | grep "关键字" ｜ grep -v grep | awk -F "" 'print $1' | xargs git tag -d

# 远程
git show-ref --tag | grep "关键字" ｜ grep -v grep | awk -F "" 'print $1' | xargs git push origin :refs/tags/
```
还没尝试，只是觉得这么可以实现，待测试。
