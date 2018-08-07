---
id: 119
title: ubuntu12.04 折腾安装RTL8723BE无线网卡驱动
date: 2015-01-14T14:44:41+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=119
permalink: /install_rtl8723be_driver_ubuntu/
categories:
  - linux
  - 极客
tags:
  - linux
  - rtl8723be
  - 无线网卡
  - 驱动
---
折腾了一上午，亲测无误，同时解决了rtl8723be不稳定总掉线的问题

首先是安装无线网卡驱动：

sudo apt-get install linux-headers-generic build-essential git

git clone http://github.com/lwfinger/rtlwifi_new
  
cd rtlwifi_new
  
make

sudo make install

&nbsp;

ubuntu 14.04的rtl8723be网卡驱动不稳定解决方法：

sudo echo &#8220;options rtl8723be fwlps=0 swlps=0&#8221; > /etc/modprobe.d/rtl8723be.conf
  
sudo reboot

有使用上的问题，请留言

&nbsp;

&nbsp;