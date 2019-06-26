---
layout: article
title:  "关于Pyinstaller的一些记录"
excerpt: "使用过程中需要注意的事情"
date:   2019-06-26 18:01:04 +0800
categories: blog
tags: [code, python]
---

#### 1. 点击启动时提示无法定位ucrtbase.abort

这种一般是`vc_2015_distributable`没有安装，对应的去windows官网下载对应32位或是64位的安装就可以。

#### 2. 提示error loading python dll

使用pyinstaller编译的时候注意添加--win-private-assemblies，一般完整的语句是
```sh
pyinstaller --clean --win-private-assemblies -F -w <python_file>
```
