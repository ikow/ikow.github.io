---
id: 64
title: pwnable.kr-lotto
date: 2016-08-09T08:34:10+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=64
permalink: /pwnable-kr-lotto/
categories:
  - pwn
tags:
  - lotto
  - pwnable.kr
---
题目描述：

> Mommy! I made a lotto program for my homework.
  
> do you want to play?
  
> ssh lotto@pwnable.kr -p2222 (pw:guest)

看下源码，是个简易的lotto系统，输入6个字符，与系统/dev/urandom生成的6个字符进行比较，如果相同的话就中奖了，但是在检查的地方代码出现了问题：

<pre class="brush: cpp; title: ; notranslate" title="">int match = 0, j = 0;
	for(i=0; i&lt;6; i++){
		for(j=0; j&lt;6; j++){
			if(lotto[i] == submit[j]){
				match++;
			}
		}
	}

</pre>

我们可以看到这里把输入的submit的每个字节都与生成的lotto的每个字节进行了比较，这里如果我们submit提交的都是同一个字节，只要lotto里面出现一次，match的值就为6，会成功返回flag，所以这里我们尝试每次都输入#######，也就是6个35：

> Submit your 6 lotto bytes : ######
  
> Lotto Start!
  
> bad luck&#8230;
  
> &#8211; Select Menu &#8211;
  
> 1. Play Lotto
  
> 2. Help
  
> 3. Exit
  
> 1
  
> Submit your 6 lotto bytes : ######
  
> Lotto Start!
  
> bad luck&#8230;
  
> &#8211; Select Menu &#8211;
  
> 1. Play Lotto
  
> 2. Help
  
> 3. Exit
  
> 1
  
> Submit your 6 lotto bytes : ######
  
> Lotto Start!
  
> sorry mom&#8230; I FORGOT to check duplicate numbers&#8230; 🙁
  
> &#8211; Select Menu &#8211;
  
> 1. Play Lotto
  
> 2. Help
  
> 3. Exit

大概尝试了三次之后成功获得了flag:

sorry mom&#8230; I FORGOT to check duplicate numbers&#8230; 🙁