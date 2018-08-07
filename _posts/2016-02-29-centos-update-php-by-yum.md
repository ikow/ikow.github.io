---
id: 145
title: CentOS/RHEL 上使用YUM安装高版本的php
date: 2016-02-29T13:11:10+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=145
permalink: /centos-update-php-by-yum/
categories:
  - linux
  - 极客
tags:
  - centos
  - install
  - PHP
  - php5.5
  - yum
  - 升级
  - 第三方
---
<img src="https://s3.amazonaws.com/awsmp-logos/centos.png" alt="" width="540" height="250" border="0" />

由于CentOS上yum默认安装的php是5.3版本，很多较新的CMS程序不支持此版本，例如Joomla! 3只支持5.4以上的版本，所以我们需要将本机的php升级到5.4以上版本。

首先卸载本机的php：

<pre class="brush:shell; toolbar: true; auto-links: true;">yum remove php  php-bcmath php-cli php-common  php-devel php-fpm    php-gd php-imap</pre>

<pre class="brush:shell; toolbar: true; auto-links: true;">php-ldap php-mbstring php-mcrypt php-mysql   php-odbc   php-pdo   php-pear  php-pecl-igbinary</pre>

<pre class="brush:shell; toolbar: true; auto-links: true;">php-xml php-xmlrpc</pre>

然后添加第三方的yum源：

<pre class="brush:shell; toolbar: true; auto-links: true;">CentOS/RHEL 7.x:
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm

CentOS/RHEL 6.x:
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
rpm -Uvh https://mirror.webtatic.com/yum/el6/latest.rpm

CentOS/RHEL 5.x:
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-5.noarch.rpm
rpm -Uvh http://mirror.webtatic.com/yum/el5/latest.rpm</pre>

注意自己的系统版本，选择对应的源进行添加。接下来就可以添加你需要版本的php了，例如php5.5:

<table class="ke-zeroborder">
  <tr>
    <td class="code">
      <pre class="bash">yum install php55w php55w-opcache</pre>
      
      <pre class="bash">你在安装的时候可能会遇到这样的问题：</pre>
      
      <pre class="brush:shell; toolbar: true; auto-links: true;">Error: php55w-common conflicts with php-common-5.3.3-46.el6_7.1.x86_64</pre>
      
      <pre class="bash">我们可以用如下方法进行解决：</pre>
      
      <pre class="brush:shell; toolbar: true; auto-links: true;">yum install yum-plugin-replace
 
yum replace php-common --replace-with=php55w-common</pre>
      
      <pre class="bash">然后再使用之前的命令安装php即可。</pre>
      
      <pre class="bash"></pre>
      
      <pre class="bash"></pre>
      
      <pre class="bash"><b>参考网站：</b><a href="https://webtatic.com/" target="_blank">https://webtatic.com/</a> （上面有各种centos升级php的文章）</pre>
      
      <pre class="bash"></pre>
      
      <pre class="bash"></pre>
      
      <pre class="bash"><b>附php package</b>(请按自己需求进行下载安装）：</pre>
      
      <table class="ke-zeroborder">
        <tr>
          <th>
            Package
          </th>
          
          <th>
            Provides
          </th>
        </tr>
        
        <tr>
          <td>
            php55w
          </td>
          
          <td>
            mod_php, php55w-zts
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-bcmath
          </td>
          
          <td>
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-cli
          </td>
          
          <td>
            php-cgi, php-pcntl, php-readline
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-common
          </td>
          
          <td>
            php-api, php-bz2, php-calendar, php-ctype, php-curl, php-date, php-exif, php-fileinfo, php-ftp, php-gettext, php-gmp, php-hash, php-iconv, php-json, php-libxml, php-openssl, php-pcre, php-pecl-Fileinfo, php-pecl-phar, php-pecl-zip, php-reflection, php-session, php-shmop, php-simplexml, php-sockets, php-spl, php-tokenizer, php-zend-abi, php-zip, php-zlib
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-dba
          </td>
          
          <td>
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-devel
          </td>
          
          <td>
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-embedded
          </td>
          
          <td>
            php-embedded-devel
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-enchant
          </td>
          
          <td>
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-fpm
          </td>
          
          <td>
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-gd
          </td>
          
          <td>
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-imap
          </td>
          
          <td>
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-interbase
          </td>
          
          <td>
            php_database, php-firebird
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-intl
          </td>
          
          <td>
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-ldap
          </td>
          
          <td>
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-mbstring
          </td>
          
          <td>
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-mcrypt
          </td>
          
          <td>
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-mssql
          </td>
          
          <td>
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-mysql
          </td>
          
          <td>
            php-mysqli, php_database
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-mysqlnd
          </td>
          
          <td>
            php-mysqli, php_database
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-odbc
          </td>
          
          <td>
            php-pdo_odbc, php_database
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-opcache
          </td>
          
          <td>
            php55w-pecl-zendopcache
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-pdo
          </td>
          
          <td>
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-pecl-gearman
          </td>
          
          <td>
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-pecl-geoip
          </td>
          
          <td>
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-pecl-memcache
          </td>
          
          <td>
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-pecl-xdebug
          </td>
          
          <td>
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-pgsql
          </td>
          
          <td>
            php-pdo_pgsql, php_database
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-process
          </td>
          
          <td>
            php-posix, php-sysvmsg, php-sysvsem, php-sysvshm
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-pspell
          </td>
          
          <td>
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-recode
          </td>
          
          <td>
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-snmp
          </td>
          
          <td>
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-soap
          </td>
          
          <td>
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-tidy
          </td>
          
          <td>
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-xml
          </td>
          
          <td>
            php-dom, php-domxml, php-wddx, php-xsl
          </td>
        </tr>
        
        <tr>
          <td>
            php55w-xmlrpc
          </td>
          
          <td>
          </td>
        </tr>
      </table>
      
      <pre class="bash">如果您在按照本文章的方法升级时遇到问题，请联系博主或者在下方留言，我将及时进行回复</pre>
    </td>
  </tr>
</table>

&nbsp;