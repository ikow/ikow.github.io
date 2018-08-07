---
id: 115
title: centos在线安装git的方法
date: 2014-12-29T15:33:55+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=115
permalink: /centos-install-git/
categories:
  - linux
tags:
  - centos
  - git
---
在安装Git之前，需要先安装一些依赖包，安装依赖包之前可以先检查下是否已经安装。

shell命令如下：

\# rpm -qa | grep zlib-devel

如果没有安装，我们先要安装这些依赖包：

\# yum -y install zlib-devel openssl-devel perl cpio expat-devel gettext-devel
  
\# yum install curl-devel
  
\# yum install autoconf
  
\# wget <a href=&#8221;http://git-core.googlecode.com/files/git-1.8.3.2.tar.gz&#8221;>http://git-core.googlecode.com/files/git-1.8.3.2.tar.gz</a>
  
\# chmod +x git-1.8.3.2.tar.gz
  
\# tar xzvf git-1.8.3.2.tar.gz
  
\# cd git-1.8.3.2
  
\# autoconf
  
\# ./configure &#8211;with-curl=/opt/git
  
\# make

\# make install

到这里git已经安装才成功了，下面我们来验证一下：

\# git &#8211;version
  
[root@AY140331154108013a0bZ git-1.8.3.2]# git version
  
git version 1.8.3.2