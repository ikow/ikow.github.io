---
id: 4496
title: windows下获取dll中函数地址的方法
date: 2018-03-22T08:59:06+00:00
author: admin
layout: post
guid: http://blog.ikow.cn/?p=4496
permalink: /get-function-location-in-dll-under-windows/
categories:
  - pwn
tags:
  - dll
  - function
  - shellcode
  - windows
  - 函数地址
---
在windows下面调试shellcode的时候需要查找一些函数的地址，这里总结下可以用的方法：

1、微软的depents工具：

http://www.dependencywalker.com/

这个在xp上应该没问题

但是在64位win7之后的系统会报错，github上有人写了新的工具解决了这个问题（https://github.com/lucasg/Dependencies）

但是我这个软件读出来的地址不是正确的函数地址，具体原因未知，待测试

&nbsp;

2、在调试的时候直接在OD中查找函数地址

使用ctrl+G 直接搜索函数名，如果有该函数会直接跳转到函数对应的地址上

这里可能会有几个问题：

1、ctrl+G 要在loadDLL之后执行，不然没有加载dll，会找不到对应函数

2、win7之后启用ASLR，每次的地址会随机加载（dll的基址随机）————>目前还没遇到这个问题

&nbsp;

3、把需要使用的api函数写个简单的c++程序，然后调试获取到最终函数的调用地址

&nbsp;

随时更新