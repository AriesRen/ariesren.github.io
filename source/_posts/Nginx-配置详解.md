---
title: Nginx 配置详解
date: 2019-05-06 11:57:56
tags: ['nginx','config']
categories: "Nginx"
---

<div style="text-align: center;">
{% note success %} 
### 前言
{% endnote %}
</div>

Nginx是Igor Sysoev为俄罗斯访问量第二的rambler.ru站点设计开发的，从2004年发布至今，凭借开源的力量，已经接近成熟和完善。Nginx功能丰富，可以作为HTTP服务器，也可以作为反向代理服务器，邮件服务器，支持FastCGI、SSL、Virtual Host、URL Rewrite、Gzip等功能，并且支持很多第三方的模块扩展。

Nginx的稳定性、功能集、实例配置文件和第系统资源的消耗也让它后来居上，在全球活跃的汪涵中有12.18%的使用比率，大约为2220万个网站。

<div style="text-align: center;">
{% note success %} 
### Nginx常用功能
{% endnote %}
</div>

**1、Http代理、反向代理**

作为web服务器最常用的功能之一，尤其是反向代理




<br>
<br>
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