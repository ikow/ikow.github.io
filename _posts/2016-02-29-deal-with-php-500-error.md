---
id: 149
title: 访问php页面出现500页面解决方法
date: 2016-02-29T13:33:47+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=149
permalink: /deal-with-php-500-error/
categories:
  - linux
  - WEB
tags:
  - "500"
  - http
  - log
  - mbstring.so
  - mb_detect_encoding
  - PHP
  - 日志
---
<img src="https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/01/11/73/metablogapi/5226.clip_image002_thumb_6114EAF6.jpg" alt="" border="0" />

经常我们在访问php的页面的时候会出现500，那么我们应该怎么解决这个问题呢？

首先，我们先来看下http  500 状态码的意义（以下摘抄自网络)：

HTTP 500 – 内部服务器错误
  
HTTP 500.100 – 内部服务器错误 – ASP 错误
  
HTTP 500-11 服务器关闭
  
HTTP 500-12 应用程序重新启动
  
HTTP 500-13 – 服务器太忙
  
HTTP 500-14 – 应用程序无效

HTTP 500-15 – 不允许请求 global.asa

我们可以看出，出现500状态码是由于服务器内部出现了问题，所以才导致了服务器无法正确返回数据进行服务。

当php网页出现的问题的时候，我们可以通过查看网站容器的日志来定位问题所在，然后解决问题：

下面是我在平时使用时遇到的两个php 500的问题，然后通过检查日志解决了问题：

**一、**

<pre class="brush:shell; toolbar: true; auto-links: true;">[Mon Feb 29 05:30:01 2016] [error] [client 104.**.**.83] Directory index forbidden by Options directive: /var/www/html/
[Mon Feb 29 05:30:02 2016] [error] [client 104.**.**.83] File does not exist: /var/www/html/favicon.ico, referer: http://*****/
[Mon Feb 29 05:30:37 2016] [error] [client 104.**.**.83] PHP Fatal error:  Call to undefined function phpinf() in /var/www/html/info.php on line 1
[Mon Feb 29 05:30:39 2016] [error] [client 104.**.**.83] PHP Fatal error:  Call to undefined function phpinf() in /var/www/html/info.php on line 1</pre>

我们可以发现，是由于info.php的代码出现了问题，我把phpinfo()函数写错了，系统无法识别该函数，导致服务器内部错误，返回500状态码，更正后，访问正常。

**二、**

<pre class="brush:shell; toolbar: true; auto-links: true;">[root@gwbtest ~]# tail /etc/httpd/logs/error_log
[Wed Jan 07 17:47:12 2015] [error] [client 192.168.3.2] PHP Fatal error:  Call to undefined</pre>

<pre class="brush:shell; toolbar: true; auto-links: true;">function mb_detect_encoding() in /var/www/phpmyadmin/libraries/php-gettext/gettext.inc on line 177</pre>

我们发现是缺少mb\_detect\_encoding()这个函数，这个是由于缺少mbstring.so文件导致的，安装php-mbstring：

<pre class="brush:shell; toolbar: true; auto-links: true;">yum install -y php54w-mbstring</pre>

（注：对于这里使用php54w，请参见[CentOS/RHEL 上使用YUM安装高版本的php](http://blog.ikow.cn/centos-update-php-by-yum)这篇文章）

安装完成后，如果还有问题的话：

到/etc/php.ini文件中添加 extension这个值，我的是

    xtension_dir = /usr/lib64/php/modules
    extension=/usr/lib64/php/modules/mbstring.so

我们可以看到当网站遇到问题了，查看日志是个很好的方法，当然前提是你开启了日志记录功能。并且日志可以帮你查出被攻击的方法和手段，说不定能抓到0day哦~~