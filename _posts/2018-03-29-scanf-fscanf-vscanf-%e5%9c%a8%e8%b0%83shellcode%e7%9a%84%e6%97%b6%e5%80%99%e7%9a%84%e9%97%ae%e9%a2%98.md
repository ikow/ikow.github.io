---
id: 4500
title: scanf/fscanf/vscanf 在调shellcode的时候的问题
date: 2018-03-29T14:46:46+00:00
author: admin
layout: post
guid: http://blog.ikow.cn/?p=4500
permalink: '/scanf-fscanf-vscanf-%e5%9c%a8%e8%b0%83shellcode%e7%9a%84%e6%97%b6%e5%80%99%e7%9a%84%e9%97%ae%e9%a2%98/'
categories:
  - pwn
tags:
  - fscanf
  - scanf
  - shellcode
  - vscanf
---
今天在调shellcode的时候遇到了一个小问题，shellcode的逻辑功能没有任何问题，但是用的时候无法执行。在OD里面调的时候发现，shellcode的后半段变成了cccc&#8230;

于是分析了下代码，发现是shellcode中有个bit是0b,然后在读取完之后变成了00cccc

> char password[1024];
  
> FILE * fp;
> 
> if(!(fp=fopen(&#8220;password.txt&#8221;,&#8221;rw+&#8221;)))
  
> {
  
> printf(&#8220;test\n&#8221;);
  
> exit(0);
  
> }
  
> fscanf(fp,&#8221;%s&#8221;,password);
> 
> 后面password会导致溢出

shellcode从password.txt中传入，最后发现是fscanf的问题导致的

上网看了下fscanf的源码发现和scanf一样都是调用vscanf，然后当以“%s&#8221;读取字符串的时候会把某些字符解析成了有含义的符号：

> <pre class="code">Hex Dec Ctrl Name
0x09  9  ^I  Tab
0x0A 10  ^J  Line Feed
0x0B 11  ^K  Verticle Feed
0x0C 12  ^L  Form Feed
0x0D 13  ^M  Carriage Return
0x1A 26  ^Z  End of File
0x20 32      Space</pre>

神奇的发现读取“00”的时候并没有问题，然后出了%s，其他几个格式化字符：

>   * <samp>%d</samp> &#8212; works fine
>   * <samp>%f</samp> &#8212; works fine
>   * <samp>%s</samp> &#8212; ignores preceeding whitespace and reads one &#8216;word&#8217;
>   * <samp>%c</samp> &#8212; rarely works as expected

如果自己在写代码的时候，尽量不要用“%s&#8221;，而是用”%c&#8221;，但是scanf/fscanf/vscanf 一般不会用在生产代码里面。

&nbsp;

那么在写shellcode的时候，如果遇到shellcode会经过scanf/fscanf/vscanf 函数处理，那么要检查下shellcode中是否有上面表格中列出来的字符。

&nbsp;

reference:

http://www.gidnetwork.com/b-64.html