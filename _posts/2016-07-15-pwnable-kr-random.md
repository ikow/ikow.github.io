---
id: 47
title: pwnable.kr-random
date: 2016-07-15T14:24:23+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=47
permalink: /pwnable-kr-random/
categories:
  - pwn
tags:
  - exploit
  - pwn
  - pwnable.kr
  - random
  - random seed
  - seed
  - shellcode
  - 溢出
---
题目描述：

> Daddy, teach me how to use random value in programming!
> 
> ssh random@pwnable.kr -p2222 (pw:guest)

其中random.c的代码为：

<pre class="brush: cpp; title: ; notranslate" title="">#include &lt;stdio.h&gt;

int main(){
 unsigned int random;
 random = rand(); // random value!

 unsigned int key=0;
 scanf("%d", &key);

 if( (key ^ random) == 0xdeadbeef ){
 printf("Good!\n");
 system("/bin/cat flag");
 return 0;
 }

 printf("Wrong, maybe you should try 2^32 cases.\n");
 return 0;
}
</pre>

代码很简单，一开始以为是需要通过key溢出覆盖random的值，结果经过调试发现每次random()生成的数值是固定的，因为在本题的代码中并没有制定随机数种子（seed），导致每次生成的第一个数都是固定的。

第一个数为：0x6b8b456，最后的结果要求是(key ^ random) == 0xdeadbeef，

所以key的值应该为：0xdeadbeef^0x6b8b4567=3039230856

输入之后，获得flag：

> random@ubuntu:~$ ./random
  
> 3039230856
  
> Good!
  
> Mommy, I thought libc random is unpredictable&#8230;

所以flag为：

Mommy, I thought libc random is unpredictable&#8230;