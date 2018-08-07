---
id: 31
title: pwnable.kr-col
date: 2016-07-08T08:51:23+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=31
permalink: /pwnable-kr-col/
categories:
  - pwn
tags:
  - col
  - exploit
  - pwn
  - pwnable.kr
  - shellcode
  - 溢出
---
首先是题目描述：

> Daddy told me about cool MD5 hash collision today.
  
> I wanna do something like that too!
> 
> ssh col@pwnable.kr -p2222 (pw:guest)

看下文件：

> col@ubuntu:~$ ls -al
  
> total 32
  
> drwxr-x&#8212;  4 root col  4096 Aug 20  2014 .
  
> dr-xr-xr-x 66 root root 4096 Jul  1 02:14 ..
  
> d&#8212;&#8212;&#8212;  2 root root 4096 Jun 12  2014 .bash_history
  
> -r-sr-x&#8212;  1 col2 col  7341 Jun 11  2014 col
  
> -rw-r&#8211;r&#8211;  1 root root  555 Jun 12  2014 col.c
  
> -r&#8211;r&#8212;&#8211;  1 col2 col2   52 Jun 11  2014 flag
  
> dr-xr-xr-x  2 root root 4096 Aug 20  2014 .irssi

col.c内容：

<pre class="brush: cpp; title: ; notranslate" title="">#include &lt;stdio.h&gt;
#include &lt;string.h&gt;
unsigned long hashcode = 0x21DD09EC;
unsigned long check_password(const char* p){
    int* ip = (int*)p;
    int i;
    int res=0;
    puts(ip);
    for(i=0; i&lt;5; i++){
        res += ip[i];
        printf("%d\n", ip[i]);
    }
    printf("%d\n",res);
    return res;
}

int main(int argc, char* argv[]){
    if(argc&lt;2){
        printf("usage : %s [passcode]\n", argv[0]);
        return 0;
    }
    if(strlen(argv[1]) != 20){
        printf("passcode length should be 20 bytes\n");
        return 0;
    }

    if(hashcode == check_password( argv[1] )){
        system("/bin/cat flag");
        return 0;
    }
    else
        printf("wrong passcode.\n");
    return 0;
}
</pre>

这题主要需要满足check\_pass返回值等于hashcode(0x21DD09EC)，check\_pass函数的功能很简单，传入一个char指针，转换成int指针，然后对数组内的数据进行求和。

这里要注意char型为1Byte，int型为4Byte，而且数据是从右向左存储：

例如我们输入1111，字符‘1’对应的ascii码的hex值为0x31，那么char型数组的内存中存储的就是31313131，变成int后对应的变量大小为0x31313131，这里换成abcdefgh更直观，传入的四个char为0xab、0xcd、0xef、0xgh，转换成int后对应的数值变成0xghefcdab，然后一共有5组这样的0xghefcdab进行求和最后的结果为0x21DD09EC,所以最后的解也是shellcode会有很多种情况了，这里我们设置四组都是相同的&#8217;0x02020202&#8217;，最后一组算出来是：

> >> hex(0x21dd09ec-0x02020202*4)
  
> &#8216;0x19d501e4&#8217;

所以反序输入应该为0xe4 0x01 0xd5 0x19

由于有非可见字符串，我们无法手动从终端输入，利用python进行输入：

> col@ubuntu:~$ ./col \`python -c &#8220;print &#8216;\x02&#8217;* 16 + &#8216;\xe4\x01\xd5\x19&#8242;&#8221;\`
  
> daddy! I just managed to create a hash collision 🙂

成功得到flag：daddy! I just managed to create a hash collision 🙂