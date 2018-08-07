---
id: 54
title: pwnable.kr-mistake
date: 2016-07-25T08:49:18+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=54
permalink: /pwnable-kr-mistake/
categories:
  - pwn
tags:
  - fd
  - mistake
  - pwnable.kr
  - read()
---
题目描述：

> We all make mistakes, let&#8217;s move on.
  
> (don&#8217;t take this too seriously, no fancy hacking skill is required at all)
> 
> This task is based on real event
  
> Thanks to dhmonkey
> 
> hint : operator priority
> 
> ssh mistake@pwnable.kr -p2222 (pw:guest)

mistake.c的源码：

<pre class="brush: cpp; title: ; notranslate" title="">#include &lt;stdio.h&gt;
#include &lt;fcntl.h&gt;

#define PW_LEN 10
#define XORKEY 1

void xor(char* s, int len){
 int i;
 for(i=0; i&lt;len; i++){
 s[i] ^= XORKEY;
 }
}

int main(int argc, char* argv[]){
 
 int fd;
 if(fd=open("/home/mistake/password",O_RDONLY,0400) &lt; 0){
 printf("can't open password %d\n", fd);
 return 0;
 }

 printf("do not bruteforce...\n");
 sleep(time(0)%20);

 char pw_buf[PW_LEN+1];
 int len;
 if(!(len=read(fd,pw_buf,PW_LEN) &gt; 0)){
 printf("read error\n");
 close(fd);
 return 0; 
 }

 char pw_buf2[PW_LEN+1];
 printf("input password : ");
 scanf("%10s", pw_buf2);

 // xor your input
 xor(pw_buf2, 10);

 if(!strncmp(pw_buf, pw_buf2, PW_LEN)){
 printf("Password OK\n");
 system("/bin/cat flag\n");
 }
 else{
 printf("Wrong Password\n");
 }

 close(fd);
 return 0;
}
</pre>

这里我们看看fd的值：

首先，当存在/home/mistake/passcode文件时，fd的返回值为0，而当fd为0时，根据[pwnable.kr-fd](http://www.ikow.cn/pwnable-kr-fd/) 我们知道，read()函数第一个参数为0时，read的值来自stdin，也就是通过命令行输入，而不是本题源代码的意思，取自passcode文件，这样，答案我们就可控了。最后我们看到pw\_buf和pw\_buf2进行比较，如果相同的话，返回正确的flag。

pw\_buf等于我们输入的内容，而pw\_buf2等于pw\_buf每一位的内容与1进行xor后的值，所以这里我们输入pw\_buf为1111111111，pw_buf1为0000000000，即可获得flag：

> mistake@ubuntu:~$ ./mistake
  
> do not bruteforce&#8230;
  
> 1111111111
  
> input password : 0000000000
  
> Password OK
  
> Mommy, the operator priority always confuses me 🙁

所以最终的flag为：

ommy, the operator priority always confuses me 🙁