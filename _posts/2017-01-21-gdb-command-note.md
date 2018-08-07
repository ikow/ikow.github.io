---
id: 4436
title: gdb命令笔记
date: 2017-01-21T16:19:25+00:00
author: admin
layout: post
guid: http://blog.ikow.cn/?p=4436
permalink: /gdb-command-note/
categories:
  - 逆向
tags:
  - gdb
  - gdb调试
---
编译的时候： -g

开始调试：gdb [-tui] test

设置断点：(gdb) breakpoint test.c:123 or  (gdb) b main

运行程序（后面可以跟参数）：(gdb) run [arg1 arg2]

清除断点：(gdb) clear

跟踪堆栈：(gdb) where

打印参数：(gdb) print f.BlockType

用16进制打印：(gdb) print/x f.BlockType

单步调试（不进入函数内部）：(gdb) next or (gdb) n

单步调试（进入函数内部）：(gdb) step or (gdb) s

在每个命令后都显示参数：(gdb) display f.BlockType

设定参数：(gdb) set f.BlockType=0

继续运行：(gdb) cont

推出：(gdb) quit