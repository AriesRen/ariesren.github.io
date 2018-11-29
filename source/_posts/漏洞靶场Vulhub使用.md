---
title: 漏洞靶场Vulhub使用
date: 2018-11-01 17:36:18
tags:
categories:
---

<div style="text-align: center;">
{% note success %} 
### 前言
{% endnote %}
</div>

Vulhub是一个面向大众的开源漏洞靶场，采用docker进行搭建，但是无需docker知识，简单执行两条命令即可编译、运行一个完整的靶场环境。该项目旨在让漏洞复现变得更加简单，让安全研究人员更专注于漏洞本身。

<div style="text-align: center;">
{% note success %} 
### 安装
{% endnote %}
</div>

我在Centos7上进行的如下步骤，如果在其他类型的机器上，可以参照进行各个环境的安装

```bash
# 安装git
yum install git
# 安装docker并启动docker
yum install docker && systemctl start docker
# 安装docker-compose
yum install docker-compose
```

由于该漏洞环境镜像均来自于Dockerhub/Github/软件官网，所以在国内访问可能会存在速度慢、丢包等问题，导致环境地洞太卡，影响正常使用，请自行解决翻墙问题，或者采用加速器进行加速。

docker-compose用户组合服务和内网，有的环境涉及到多个容器、端口等，docker-compose可以做到环境的一键化管理，用户不需要再学习各种参数和用法，只需要简单的执行`docker-compose up -d`即可启动容器环境。

安装完上述环境之后，可以通过以下命令来下载vulhub环境到任何目录

```bash
git clone https://github.com/vulhub/vulhub.git
```

<div style="text-align: center;">
{% note success %} 
### 启动漏洞环境
{% endnote %}
</div>

docker-compose会自动查找当前目录下的配置文件(默认文件名为docker-compose.xml),并根据其内容编译镜像和启动容器。所以，要运行某个漏洞靶场，需要先进入该漏洞所在的目录。

在vulhub中选择某个环境，进入对应目录。如Flask服务端模板注入漏洞，我们进入`flask/ssti`目录，执行如下命令，进行漏洞靶场的编译和运行：
```bash
cd flask/ssti
docker-compose build
docker-compose up -d
```



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