---
layout: article
title:  "为了使用Jekyll我做了什么"
excerpt: "一些步骤的小记录"
date:   2019-06-24 18:01:04 +0800
categories: blog
tags: [code, blog]
background: '/assets/img/purple-pink.png'
---

### 1. 安装

在windows下直接去下载ruby的安装包就好了，在ubuntu下面可以
```bash
sudo apt install ruby-dev ruby
```
完成之后应该会同步装上gem。

为了避免某些时候下载缓慢，可以换成中国的镜像源
```bash
gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
gem sources -l
# https://gems.ruby-china.com
# 确保只有 gems.ruby-china.com
```

然后就可以比较爽快地安装bundle和jekyll了
```bash
gem install bundle jekyll
```

安装完之后把bundle的源也换一下
```sh
bundle config mirror.https://rubygems.org https://gems.ruby-china.com
```
这样子后面`bundle install`也会变得非常舒爽。

### 2. 模板（不想从头开始）

去找一个theme下载好，进到该目录，`bundle install`之后`jekyll serve`再访问localhost:4000就可以看到模板的样子。

梳理一下jekyll必须的文件结构：

```bash
blog
│  404.html # 404页面
│  about.md # about页面
│  contact.md # contact信息
│  Gemfile # Gem配置
│  Gemfile.lock
│  index.md # home啦
│  _config.yml # jekyll配置文件
│
├─.sass-cache # 一些样式文件
│
├─assets
│  ├─img
│  └─vendor
│
├─posts # 在这里同jekyll-posinate-v2生成了一个posts的目录
│      index.html
│      
├─_includes # 一些head, footer的模板
│      
├─_layouts #post、page等等的生成模板
│      
├─_posts #写日志的地方
│   └─2019-06-23-Bobo-First-Blog.md
│      
└─_site #自动生成的html页面
    ├─blog
    │  └─2019
    │      └─06
    │          └─23
    │                  Bobo-First-Blog.html
    │                  
    └─posts
            index.html
```

### 3. 样式

自己做了一个css来控制图片和默认字体的大小。
```css
p{
    font-size: 1.1rem;
}

img{
    width:100%;
}
```