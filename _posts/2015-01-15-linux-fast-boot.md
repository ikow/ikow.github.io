---
id: 121
title: linux下快捷启动
date: 2015-01-15T22:00:36+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=121
permalink: /linux-fast-boot/
categories:
  - linux
  - 极客
tags:
  - bash
  - linux
---
我这里指的快捷启动就是，我在终端在输入qq，那么就能直接启动qq程序

首先第一步，把对应的xxoo程序所有文件放到：/opt/xxoo文件夹

然后设置对应程序的权限sudo chmod +x xxoo.sh

在/usr/bin/ 建立对应文件 ：sudo vim /usr/bin/xxoo

内容为：

#！/bin/bash

/opt/xxoo/xxoo.sh

设置运行权限： udo chmod +x /usr/bin/xxoo

最后在终端在运行sudo xxoo就可以实现便捷启动啦～～～

&nbsp;