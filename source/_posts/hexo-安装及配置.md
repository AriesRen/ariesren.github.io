---
title: hexo-安装及配置
date: 2018-09-07 18:39:40
tags: ['hexo','博客', 'nexT','Github Pages']
categories: hexo
---

## 前言
曾几何时，你是否也想有个自己的博客，抒发自己的心情，总结自己的得失，与人分享喜悦、哀伤、愤怒、忧愁，那么这篇文章你就必须看了，非常简单搭建一个自己的开源博客。

## 一、预备
  
**1、安装Nodejs及npm**

Nodejs下载地址： [官网下载地址：https://nodejs.org/zh-cn/download/](https://nodejs.org/zh-cn/download/)

**2、安装Git**

Git下载地址： [官网下载地址：https://git-scm.com/download/](https://git-scm.com/download/)

安装完成后，执行如下命令，可以显示版本号就算安装成功了  

```shell
$ node -v
v9.11.1

$ npm -v
6.3.0

$ git --version
git version 2.17.0.windows.1
```

## 二、安装hexo 

进入命令行，执行如下命令:  

```shell
1、全局安装hexo
$ npm install hexo -g

2、创建hexo工作目录
$ mkdir hexo-blog
$ cd hexo-blog

3、初始化工作目录
$ hexo init

4、本地启动hexo
$ hexo serve
```
  
到此一个hexo博客已经搭建完成了，可以访问 http://localhost:4000/ 查看博客的效果。

<br>

当然现在你就可以开始写博客了，默认的配置足够你写作、发表文章了，但是默认的东西有些并不符合自己的要求和审美。所以下面对hexo进行一些配置，以符合自己的要求。

## 三、hexo配置

hexo的配置文件在根目录下_config.yml文件中。本文仅列举几项，其余配置可以参照[hexo官网文档](https://hexo.io/zh-cn/docs/configuration.html)进行配置，当然，有兴趣可以参照[我的配置](https://github.com/AriesRen/ariesren.github.io)


网站配置：
```yml
# Site
title: Aries' blog 网站标题
subtitle: 副标题
description: 我不生产知识，我只是知识的搬运工。 网站一句话描述
keywords: 关键词
author: 无名万物 作者
language: zh-CN 语言
timezone: Asia/Shanghai 时区
```

文章配置： 
```yml
url: http://blog.renhj.org  网站url
root: /   文章根路径
permalink: posts/:year-:month-:day-:title.html  文章url
permalink_defaults:
```

## 四、创建新文章

你可以通过以下命令来创建一篇新文章
```shell
hexo new [layout] <title>
```

命令中指令文章的布局，默认为post，可以通过修改_config.yml中的default_layout来修改默认布局，当然也可以在文章Front-Matter上添加布局.


当然也可以新建一个草稿： draft，这种布局在建立时会保存到`source/_drafts`文件夹，也可以通过`publish`来将草稿移动到正式文件夹。

```shell
# 新建草稿文章
$ hexo new draft <title>

# 将文章正式发布
$ hexo publish [layout] <title>
```

**Front-matter**

Front-matter是文章最上方以`---` 分割的区域，用于指定个别文件的变量

```
---
layout: 指定文章的布局属性
title： 文章标题
data：建立日期
updated： 更新日期
comments： 是否开启文章的评论功能(如果有的话)
tags： 标签
categories：分类
permalink： 覆盖文章的网址
---
```


##  修改美化


默认的主题是有点丑，可以去[hexo的主题商店](https://hexo.io/themes/) 找一个自己喜欢的、漂亮的主题。

本人找的是网上比较流行的nexT的主题，即本博客所使用的主题：[hexo nexT主题](http://theme-next.iissnan.com/)，更多的配置可以参照nexT官网的配置或者其他文章进行配置。本文就不再这里赘述的，具体效果可以看本博客的。

<br>

<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- 信息流广告 -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4127326375481893"
     data-ad-slot="9105526840"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>