---
id: 43
title: pwnable.kr-flag
date: 2016-07-11T07:45:42+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=43
permalink: /pwnable-kr-flag/
categories:
  - pwn
tags:
  - crack
  - pwn
  - pwnable.kr
  - upx
  - 脱壳
  - 逆向
---
题目描述：

> Papa brought me a packed present! let&#8217;s open it.
> 
> Download : http://pwnable.kr/bin/flag
> 
> This is reversing task. all you need is binary

下载下来之后file一下：

> ➜ Desktop file flag
  
> flag: ELF 64-bit LSB executable, x86-64, version 1 (GNU/Linux), statically linked, stripped

64位的ELF，直接拖到IDA里面看看，发现只有三个函数，而且并不能正常打开，目测是加壳了。于是在里面瞎翻，发现了upx的关键字，果断upx -d flag把壳脱了。重新拖进IDA，发现代码很简单：

<pre class="brush: cpp; title: ; notranslate" title="">int __cdecl main(int argc, const char **argv, const char **envp)
{
  __int64 v3; // rax@1

  puts("I will malloc() and strcpy the flag there. take it.", argv, envp);
  LODWORD(v3) = malloc(100LL);
  sub_400320(v3, flag);
  return 0;
}
</pre>

malloc申请了一个100LL的地址，然后把flag复制进去了，直接查看flag的值，发现直接出现flag了：

UPX&#8230;? sounds like a delivery service 🙂