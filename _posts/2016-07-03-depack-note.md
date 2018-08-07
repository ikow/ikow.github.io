---
id: 153
title: 逆向脱壳小记
date: 2016-07-03T13:16:25+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=153
permalink: /depack-note/
categories:
  - 逆向
tags:
  - 加壳
  - 脱壳
  - 逆向
---
PECompact 2.X壳

壳的前两句是mov eax,xxxxxxxx   push eax

记下xxxxxxx的地址，ctrl+G跳转到该地址，然后向下找jmp eax

F4断点，然后F8单步进入就能找到OEP了