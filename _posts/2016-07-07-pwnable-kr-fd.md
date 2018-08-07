---
id: 23
title: pwnable.kr-fd
date: 2016-07-07T16:24:02+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=23
permalink: /pwnable-kr-fd/
categories:
  - pwn
tags:
  - exploit
  - pwn
  - pwnable.kr
  - 溢出
---
题目描述：

> Mommy! what is a file descriptor in Linux?
> 
> ssh fd@pwnable.kr -p2222 (pw:guest)

连上后查看下信息：

> fd@ubuntu:~$ ls -al
  
> total 36
  
> drwxr-x&#8212;  4 root fd   4096 Jul  1 02:36 .
  
> dr-xr-xr-x 66 root root 4096 Jul  1 02:14 ..
  
> d&#8212;&#8212;&#8212;  2 root root 4096 Jun 12  2014 .bash_history
  
> -r-sr-x&#8212;  1 fd2  fd   7322 Jun 11  2014 fd
  
> -rw-r&#8211;r&#8211;  1 root root  418 Jun 11  2014 fd.c
  
> -r&#8211;r&#8212;&#8211;  1 fd2  root   50 Jun 11  2014 flag
  
> -rw&#8212;&#8212;-  1 root root   80 May 29 09:13 .gdb_history
  
> dr-xr-xr-x  2 root root 4096 Aug 20  2014 .irssi

fd.c的代码如下：

<pre class="brush: cpp; title: ; notranslate" title="">#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
char buf[32];
int main(int argc, char* argv[], char* envp[]){
if(argc&lt;2){
printf("pass argv[1] a number\n");
return 0;
}
int fd = atoi( argv[1] ) - 0x1234;
int len = 0;
len = read(fd, buf, 32);
if(!strcmp("LETMEWIN\n", buf)){
printf("good job :)\n");
system("/bin/cat flag");
exit(0);
}
printf("learn about Linux file IO\n");
return 0;

}
</pre>

我们需要执行到system(“/bin/cat flag”);
  
需要满足strcmp(“LETMEWIN\n”, buf) == 0
  
buf = “LETMEWIN\n”
  
通过read(fd, buf, 32)将buf设为”LETMEWIN\n”

看一下read()函数的参数：

fd == 0为标准输入
  
fd == 1为标准输出
  
fd == 2为标准错误输出
  
所以这里我们只要使fd == 0，然后就可以通过终端输入LETMEWIN后回车
  
要使fd == 0，
  
则：输入的参数 == 0x1234，即4660

所以操作如下：

> fd@ubuntu:~$ ./fd 4660
  
> LETMEWIN
  
> good job 🙂
  
> mommy! I think I know what a file descriptor is!!

所以最终的flag为：mommy! I think I know what a file descriptor is!!