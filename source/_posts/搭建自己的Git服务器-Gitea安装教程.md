---
title: 搭建自己的Git服务器-Gitea安装教程
date: 2020-02-26 19:04:49
tags: ['gitea', 'git']
categories: 
---

# 0x01 为啥要费劲的自己搭一个git服务器

本人空闲时间喜欢自己鼓捣一些项目，但是又不喜欢在本地存，所有都放到github上了，最近去Github看了一下，又多又乱，github俨然成了一个demohub了，所以有了建一个自己的代码仓库管理自己代码的想法。另一方面，也由于Github的访问速度有点慢，所以说干就干，在自己的服务器上搭了这么一个gitea服务器。

调查了一些市面上的Git服务器，大概有这么三个gogs、gitlab、gitea。

* gogs是使用go语言开发的一个轻量级的git服务器。
* gitlab算是目前企业使用较多，也比较火的一个git自建服务应用，但是对于个人来说，服务器的配置可能跟不上。
* gitea是gogs的一个克隆版本，但是相比较gogs，是采用社区进行维护的，问题解决的也比较及时。

因此我这里用gitea搭建了一个自己的git服务器，用来作为自己的一个代码仓库，一个方便管理，另一个是~~方便装逼~~。

# 0x02 准备工作

我自己搭建的使用的存储数据库采用的是mysql，当然可以根据gitea的文档自己选择和搭建。

这里放一下[gitea的官网](https://docs.gitea.io/en-us/database-prep/)，有需要的可以参照英文文档步骤自己搭建。

首先要安装mysql数据库，由于之前我已经安装过了，这里就不做安装步骤的，有需要的自己上网搜索。

### Mysql

1、 以root用户登录数据库。

```bash
[root@renhj ~]# mysql -u root -p
Enter password:
```

2、 创建gitea所需的数据库用户，这里我用的用户名为gitea，密码为!QAZ2wsx， 你可以用你熟悉的比较安全的密码。

如果你的gitea和mysql是在同一台服务器上，按照下面的创建：

```bash
mysql> CREATE USER 'gitea' IDENTIFIED BY '!QAZ2wsx';
```

对于不在一台服务器上的，则可以按照下面的语句进行创建：
```bash
mysql> CREATE USER 'gitea'@'%' IDENTIFIED BY '!QAZ2wsx';
```

`%`是对于所有外部服务器进行匹配的，如果你只想限定某一台服务器登录mysql，可以把`%`换成你的gitea服务器的IP地址。

3、创建gitea所需的数据库giteadb，编码用utf8mb4.

```bash
mysql> CREATE DATABASE giteadb CHARACTER SET 'utf8mb4' COLLATE 'utf8mb4_unicode_ci';
```

4、授权数据库的权限。

git服务和mysql在一台服务器上的：

```bash
mysql> GRANT ALL PRIVILEGES ON giteadb.* TO 'gitea';
```
git服务和mysql不在一台服务器上的：

```bash
mysql> GRANT ALL PRIVILEGES ON giteadb.* TO 'gitea'@'%';
```

可以把`%`换成你的gitea服务器的IP地址。

```bash
mysql> FLUSH PRIVILEGES;
```

## Git

检查服务器上是否安装了git

```bash
[root@renhj ~]# git --version
git version 1.8.3.1
```

如果没有安装的话，用yum或者二进制包进行安装

```bash
[root@renhj ~]# sudo yum install git -y
```

## 系统

1、创建运行gitea服务的系统用户

```bash
[root@renhj ~]# groupadd git
[root@renhj ~]# adduser --system --shell /bin/bash --comment 'Git Version Control' --user-group --home-dir /home/git -m git
```

2、创建运行所需的目录

```bash
[root@renhj ~]# mkdir -p /var/lib/gitea/{custom,data,log}
[root@renhj ~]# chown git:git /var/lib/gitea/{data,indexers,log}
[root@renhj ~]# mkdir /etc/gitea
[root@renhj ~]# chown root:git /etc/gitea/
[root@renhj ~]# chmod 770 /etc/gitea/
```

3、下载二进制文件

```bash
[root@renhj ~]# mkdir /usr/local/bin/
[root@renhj ~]# cd /usr/local/bin/
[root@renhj bin]# wget -O gitea https://dl.gitea.io/gitea/1.9.6/gitea-1.9.6-linux-amd64
[root@renhj bin]# chmod +x gitea
```

4、将gitea做成服务

```bash
[root@renhj ~]# vim /etc/systemd/system/gitea.service
[Unit]
Description=Gitea (Git with a cup of tea)
After=network.target
Requires=mysqld.service

[Service]
RestartSec=2s
Type=simple
User=git
Group=git
WorkingDirectory=/var/lib/gitea/
ExecStart=/usr/local/bin/gitea web -c /etc/gitea/app.ini
Restart=always
Environment=USER=git HOME=/home/git GITEA_WORK_DIR=/var/lib/gitea

[Install]
WantedBy=multi-user.target
```
5、启动gitea

```bash
[root@renhj ~]# systemctl daemon-reload 
[root@renhj ~]# systemctl enable gitea
[root@renhj ~]# systemctl start gitea
```

启动之后可以通过http://[you ip]:3000进行访问，或者可以通过nginx做一个反向代理，nginx的安装这里就不写了，我把反向代理的配置贴出来。

6、nginx配置反向代理

```bash
[root@renhj bin]# vim /etc/nginx/conf.d/gitea.conf 
server {
	listen 80;
	server_name git.renhj.org;

	location / {
		proxy_pass http://127.0.0.1:3000/;
		proxy_set_header Host $http_host;
		proxy_set_header X-Forwarded-For $remote_addr;
		proxy_set_header Upgrade $http_upgrade;
		proxy_set_header Connection $http_connection;	
	}
}
```

同时不要忘了在你的域名解析上加上一条git的解析。

7、注册git管理员及配置

这里已经算是安装完成了，但是gitea的管理员是没有的，数据库其他配置也没有配置。

通过访问`/install`页面进行配置，这里我的地址是`http://git.renhj.org/install`

我这里已经配置完成了，所以就不展示了，这里你配置上自己的数据库信息就行之，之后会让你进行注册，第一个注册的用户是管理员账户。



8、配置仓库地址

我查了很多网上的教程，很多教程到上一步就结束了，其实不是。还需要配置一下git代码的地址，不然你git推送和拉取都是localhost的地址。

将/etc/gitea/app.ini文件里server标签下的SSH_DOMAIN、DOMAIN、ROOT_URL都改为你自己配置的域名如下。

```bash
[root@renhj bin]# vim /etc/gitea/app.ini 
[server]
SSH_DOMAIN       = git.renhj.org
DOMAIN           = git.renhj.org
HTTP_PORT        = 3000
ROOT_URL         = http://git.renhj.org/
```

当然还有一些配置如禁止注册、邮箱配置，我这里也不需要，就不配置了。如果你需要可以到官网上去找，或者联系我。



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