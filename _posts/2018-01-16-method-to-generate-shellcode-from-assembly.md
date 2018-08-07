---
id: 4493
title: 从汇编生成shellcode的n种方法
date: 2018-01-16T14:58:09+00:00
author: admin
layout: post
guid: http://blog.ikow.cn/?p=4493
permalink: /method-to-generate-shellcode-from-assembly/
categories:
  - pwn
tags:
  - asm
  - gas
  - nasm
  - pwn
  - shellcode
---
第一种，添加asm代码到c中，然后gcc编译生成可执行代码，最后objdump：

void main() {

asm{

&#8230;

}

}

太麻烦，这里就不详细介绍了，基本上包含在第二种方法中

&nbsp;

第二种，直接用NASM或者GAS生成elf文件，然后objdump：

> nasm -f elf print.asm
  
> ld -m elf_i386 -o print print.asm
> 
> as test.asm -o test.o
  
> ld test.asm -o test

objdump生成shellcode：

> objdump -d print2 | grep &#8220;^ &#8221; | cut -d$&#8217;\t&#8217; -f 2 | tr &#8216;\n&#8217; &#8216; &#8216; | sed -e &#8216;s/ *$//&#8217; | sed -e &#8216;s/ \+/\\x/g&#8217; | awk &#8216;{print &#8220;\\x&#8221;$0}&#8217;

关于NASM和GAS的区别可以看：

https://www.ibm.com/developerworks/library/l-gas-nasm/

&nbsp;

第三种，使用pwntools（https://github.com/Gallopsled/pwntools#readme）

example：

<pre class="brush: python; title: ; notranslate" title="">from pwn import *

code = """.global _start
_start:
        jmp     test1
test2:
        pop     ebx
        mov     al, 0xa
        int     0x80
        mov     al, 0x1
        xor     ebx, ebx
        int     0x80
test1:
        call    test2
        .string "delfile" """

context(arch='x86', os='linux', endian='little', word_size=32)
shellcode = asm(code).encode('hex')
re = ''
while len(shellcode):
    re += r'\x'+shellcode[:2]
    shellcode = shellcode[2:]
print re&lt;span data-mce-type="bookmark" style="display: inline-block; width: 0px; overflow: hidden; line-height: 0;" class="mce_SELRES_start"&gt;&lt;/span&gt;
</pre>

&nbsp;

未完待补充

&nbsp;