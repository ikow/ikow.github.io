---
id: 100
title: kali linux(debian)安装无线网卡驱动
date: 2014-08-10T22:50:18+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=100
permalink: /wireless_driver_for_linux/
categories:
  - linux
  - 极客
tags:
  - driver
  - kali
  - linux
  - Wi-Fi
  - 驱动
---
<img src="http://kowapp.u.qiniudn.com/kali.jpg" alt="" border="0" />

换了新电脑就想折腾一下，装个kali linux，然后破解一下家周围的wifi，顺便可以劫持个妹子的qq号啥的（YY流口水中。。）

好吧，回到正题，按照kali官方说明从U盘装完双系统后（原来是win7 64bit），点网络链接只有有线连接和vpn连接可以使用，插上网线，有网络访问，先一顿“apt-get update ,apt-get upgrade”，换源啥的。最后回到开始装kali的目的，用工具破解wifi密码（尼玛，装了半天 正事没有完成瞎折腾啥呀）

系统环境：

<pre class="brush:shell; toolbar: true; auto-links: true;">root@kow:~# uname -a
Linux kow 3.14-kali1-amd64 #1 SMP Debian 3.14.5-1kali1 (2014-06-07) x86_64 GNU/Linux</pre>

百度了一下，发现debian装驱动需要自己下载然后编译安装，那就老老实实按网上的步骤来吧

到官网下驱动，我的无线网卡型号是：RTL8723BE，结果到官网没找到驱动（可能太笨了吧，如果你找到了 email我一下是怎么找到的），然后在其他软件站上下载了一个。

解压，cd 进目录，make 好吧这步就出错了，首先是报错：

没有**kernel-source吧，一开始编译就出问题，然后键入：**

<pre class="brush:shell; toolbar: true; auto-links: true;"><strong>apt-get install linux-headers-$(uname -r)</strong></pre>

**第一个错误搞定。**

&nbsp;

**       ** 然后又出了一堆什么warning 认为成error，这个改了半天也没搞定（应该是编译器设置的问题），提示

<pre>_ieee80211_is_robust_mgmt_frame()这个函数没有，百度了一下，发现了这篇文章：</pre>

<a href="https://wiki.archlinux.org/index.php/Wireless_Setup_%28%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%29" target="_blank">https://wiki.archlinux.org/index.php/Wireless_Setup_%28%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%29</a>

按照上面的方法，顺利解决问题，安装好了驱动，可以愉快的破解别人wifi了

写在最后：

吐槽一下国内的各种论坛博客，大多数人的问题要么下面没人回答，要么乱回答，你回复问题好歹你自己先试一下啊，语句都是错的。

&nbsp;