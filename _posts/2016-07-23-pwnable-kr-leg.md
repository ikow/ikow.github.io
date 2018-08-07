---
id: 52
title: pwnable.kr-leg
date: 2016-07-23T14:43:24+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=52
permalink: /pwnable-kr-leg/
categories:
  - pwn
tags:
  - arm
  - arm指令
  - bx
  - exploit
  - lr
  - pc
  - pwnable.kr
  - r0
  - thumb
---
题目描述：

> Daddy told me I should study arm.
  
> But I prefer to study my leg!
> 
> Download : http://pwnable.kr/bin/leg.c
  
> Download : http://pwnable.kr/bin/leg.asm
> 
> ssh leg@pwnable.kr -p2222 (pw:guest)

这题的描述比较有意思，这题主要考察arm的汇编指令，当然此ARM非彼arm（胳膊），leg.c的代码：

<pre class="brush: cpp; title: ; notranslate" title="">#include &amp;amp;amp;amp;lt;stdio.h&amp;amp;amp;amp;gt;
#include &amp;amp;amp;amp;lt;fcntl.h&amp;amp;amp;amp;gt;
int key1(){
 asm("mov r3, pc\n");
}
int key2(){
 asm(
 "push {r6}\n"
 "add r6, pc, $1\n"
 "bx r6\n"
 ".code 16\n"
 "mov r3, pc\n"
 "add r3, $0x4\n"
 "push {r3}\n"
 "pop {pc}\n"
 ".code 32\n"
 "pop {r6}\n"
 );
}
int key3(){
 asm("mov r3, lr\n");
}
int main(){
 int key=0;
 printf("Daddy has very strong arm! : ");
 scanf("%d", &amp;amp;amp;amp;amp;key);
 if( (key1()+key2()+key3()) == key ){
 printf("Congratz!\n");
 int fd = open("flag", O_RDONLY);
 char buf[100];
 int r = read(fd, buf, 100);
 write(0, buf, r);
 }
 else{
 printf("I have strong leg :P\n");
 }
 return 0;
}
</pre>

leg.asm的代码：

<pre class="brush: cpp; title: ; notranslate" title="">(gdb) disass main
Dump of assembler code for function main:
 0x00008d3c &lt;+0&gt;: push {r4, r11, lr}
 0x00008d40 &lt;+4&gt;: add r11, sp, #8
 0x00008d44 &lt;+8&gt;: sub sp, sp, #12
 0x00008d48 &lt;+12&gt;: mov r3, #0
 0x00008d4c &lt;+16&gt;: str r3, [r11, #-16]
 0x00008d50 &lt;+20&gt;: ldr r0, [pc, #104] ; 0x8dc0 &lt;main+132&gt;
 0x00008d54 &lt;+24&gt;: bl 0xfb6c &lt;printf&gt;
 0x00008d58 &lt;+28&gt;: sub r3, r11, #16
 0x00008d5c &lt;+32&gt;: ldr r0, [pc, #96] ; 0x8dc4 &lt;main+136&gt;
 0x00008d60 &lt;+36&gt;: mov r1, r3
 0x00008d64 &lt;+40&gt;: bl 0xfbd8 &lt;__isoc99_scanf&gt;
 0x00008d68 &lt;+44&gt;: bl 0x8cd4 &lt;key1&gt;
 0x00008d6c &lt;+48&gt;: mov r4, r0
 0x00008d70 &lt;+52&gt;: bl 0x8cf0 &lt;key2&gt;
 0x00008d74 &lt;+56&gt;: mov r3, r0
 0x00008d78 &lt;+60&gt;: add r4, r4, r3
 0x00008d7c &lt;+64&gt;: bl 0x8d20 &lt;key3&gt;
 0x00008d80 &lt;+68&gt;: mov r3, r0
 0x00008d84 &lt;+72&gt;: add r2, r4, r3
 0x00008d88 &lt;+76&gt;: ldr r3, [r11, #-16]
 0x00008d8c &lt;+80&gt;: cmp r2, r3
 0x00008d90 &lt;+84&gt;: bne 0x8da8 &lt;main+108&gt;
 0x00008d94 &lt;+88&gt;: ldr r0, [pc, #44] ; 0x8dc8 &lt;main+140&gt;
 0x00008d98 &lt;+92&gt;: bl 0x1050c &lt;puts&gt;
 0x00008d9c &lt;+96&gt;: ldr r0, [pc, #40] ; 0x8dcc &lt;main+144&gt;
 0x00008da0 &lt;+100&gt;: bl 0xf89c &lt;system&gt;
 0x00008da4 &lt;+104&gt;: b 0x8db0 &lt;main+116&gt;
 0x00008da8 &lt;+108&gt;: ldr r0, [pc, #32] ; 0x8dd0 &lt;main+148&gt;
 0x00008dac &lt;+112&gt;: bl 0x1050c &lt;puts&gt;
 0x00008db0 &lt;+116&gt;: mov r3, #0
 0x00008db4 &lt;+120&gt;: mov r0, r3
 0x00008db8 &lt;+124&gt;: sub sp, r11, #8
 0x00008dbc &lt;+128&gt;: pop {r4, r11, pc}
 0x00008dc0 &lt;+132&gt;: andeq r10, r6, r12, lsl #9
 0x00008dc4 &lt;+136&gt;: andeq r10, r6, r12, lsr #9
 0x00008dc8 &lt;+140&gt;: ; &lt;UNDEFINED&gt; instruction: 0x0006a4b0
 0x00008dcc &lt;+144&gt;: ; &lt;UNDEFINED&gt; instruction: 0x0006a4bc
 0x00008dd0 &lt;+148&gt;: andeq r10, r6, r4, asr #9
End of assembler dump.
(gdb) disass key1
Dump of assembler code for function key1:
 0x00008cd4 &lt;+0&gt;: push {r11} ; (str r11, [sp, #-4]!)
 0x00008cd8 &lt;+4&gt;: add r11, sp, #0
 0x00008cdc &lt;+8&gt;: mov r3, pc
 0x00008ce0 &lt;+12&gt;: mov r0, r3
 0x00008ce4 &lt;+16&gt;: sub sp, r11, #0
 0x00008ce8 &lt;+20&gt;: pop {r11} ; (ldr r11, [sp], #4)
 0x00008cec &lt;+24&gt;: bx lr
End of assembler dump.
(gdb) disass key2
Dump of assembler code for function key2:
 0x00008cf0 &lt;+0&gt;: push {r11} ; (str r11, [sp, #-4]!)
 0x00008cf4 &lt;+4&gt;: add r11, sp, #0
 0x00008cf8 &lt;+8&gt;: push {r6} ; (str r6, [sp, #-4]!)
 0x00008cfc &lt;+12&gt;: add r6, pc, #1
 0x00008d00 &lt;+16&gt;: bx r6
 0x00008d04 &lt;+20&gt;: mov r3, pc
 0x00008d06 &lt;+22&gt;: adds r3, #4
 0x00008d08 &lt;+24&gt;: push {r3}
 0x00008d0a &lt;+26&gt;: pop {pc}
 0x00008d0c &lt;+28&gt;: pop {r6} ; (ldr r6, [sp], #4)
 0x00008d10 &lt;+32&gt;: mov r0, r3
 0x00008d14 &lt;+36&gt;: sub sp, r11, #0
 0x00008d18 &lt;+40&gt;: pop {r11} ; (ldr r11, [sp], #4)
 0x00008d1c &lt;+44&gt;: bx lr
End of assembler dump.
(gdb) disass key3
Dump of assembler code for function key3:
 0x00008d20 &lt;+0&gt;: push {r11} ; (str r11, [sp, #-4]!)
 0x00008d24 &lt;+4&gt;: add r11, sp, #0
 0x00008d28 &lt;+8&gt;: mov r3, lr
 0x00008d2c &lt;+12&gt;: mov r0, r3
 0x00008d30 &lt;+16&gt;: sub sp, r11, #0
 0x00008d34 &lt;+20&gt;: pop {r11} ; (ldr r11, [sp], #4)
 0x00008d38 &lt;+24&gt;: bx lr
End of assembler dump.
(gdb) 
</pre>

这题的代码其实很简单，我们输入的key，需要让他满足其值为key1(), key2(), key3()三个函数返回值的和，这里需要对三个函数分别进行分析：

首先是key1:

<pre>Dump of assembler code for function key1:
 0x00008cd4 &lt;+0&gt;: push {r11} ; (str r11, [sp, #-4]!)
 0x00008cd8 &lt;+4&gt;: add r11, sp, #0
 0x00008cdc &lt;+8&gt;: mov r3, pc
 0x00008ce0 &lt;+12&gt;: mov r0, r3
 0x00008ce4 &lt;+16&gt;: sub sp, r11, #0
 0x00008ce8 &lt;+20&gt;: pop {r11} ; (ldr r11, [sp], #4)
 0x00008cec &lt;+24&gt;: bx lr</pre>

在ARM汇编中，子函数通常是通过寄存器r0返回函数的返回值，所以这里我们看r0的值，r0等于r3，r3等于pc的值，关于pc的说明，请见：http://blog.sina.com.cn/s/blog_bcdac52b0101nf7j.html

所以这里r0也就是key1的值为0x8cdc+8

接着是key2:

<pre>Dump of assembler code for function key2:
 0x00008cf0 &lt;+0&gt;: push {r11} ; (str r11, [sp, #-4]!)
 0x00008cf4 &lt;+4&gt;: add r11, sp, #0
 0x00008cf8 &lt;+8&gt;: push {r6} ; (str r6, [sp, #-4]!)
 0x00008cfc &lt;+12&gt;: add r6, pc, #1
 0x00008d00 &lt;+16&gt;: bx r6
 0x00008d04 &lt;+20&gt;: mov r3, pc
 0x00008d06 &lt;+22&gt;: adds r3, #4
 0x00008d08 &lt;+24&gt;: push {r3}
 0x00008d0a &lt;+26&gt;: pop {pc}
 0x00008d0c &lt;+28&gt;: pop {r6} ; (ldr r6, [sp], #4)
 0x00008d10 &lt;+32&gt;: mov r0, r3
 0x00008d14 &lt;+36&gt;: sub sp, r11, #0
 0x00008d18 &lt;+40&gt;: pop {r11} ; (ldr r11, [sp], #4)
 0x00008d1c &lt;+44&gt;: bx lr
End of assembler dump.</pre>

key2中r0的值也是来自r3，r3等于偏移20处的pc的值，然后下一行再加上4，这里需要注意的是，这里有一个bx，关于bx，请见：http://blog.csdn.net/liuchao1986105/article/details/6539728

在c代码中我们也可以看到.code的伪指令，来表明是ARM指令和thumb指令之间的切换，所以这里的pc统统都变成了当前地址+4，因为指令集由ARM的32位的指令变成16指令，对应的指令长度变成了原来的一半，所以这里r0也就是key2的值为0x8d04+4+4

最后是key3：

<pre>Dump of assembler code for function key3:
 0x00008d20 &lt;+0&gt;: push {r11} ; (str r11, [sp, #-4]!)
 0x00008d24 &lt;+4&gt;: add r11, sp, #0
 0x00008d28 &lt;+8&gt;: mov r3, lr
 0x00008d2c &lt;+12&gt;: mov r0, r3
 0x00008d30 &lt;+16&gt;: sub sp, r11, #0
 0x00008d34 &lt;+20&gt;: pop {r11} ; (ldr r11, [sp], #4)
 0x00008d38 &lt;+24&gt;: bx lr
End of assembler dump.</pre>

这里r0等于r3就是等于lr的值，关于lr指令，一般它的值等于：1、子函数的返回地址；2、发生异常时pc-4，这里lr保证了程序能够正常运行，由于这里程序并没有异常，所以这里保存的是key3()函数的返回地址，也就是：0x8d80

所以最终key的值应该为0x8cdc+8+0x8d04+4+4+0x8d80=108400

然后输入这个key，就能成功获得flag：

> / $ ./leg
  
> Daddy has very strong arm! : 108400
  
> Congratz!
  
> My daddy has a lot of ARMv5te muscle!

最终的flag为：

My daddy has a lot of ARMv5te muscle!