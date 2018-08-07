---
id: 157
title: gdb peda常用指令
date: 2016-07-14T16:13:04+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=157
permalink: /gdb-peda-common-command/
categories:
  - 逆向
tags:
  - gdb
  - gdb调试
  - peda
  - 常用指令
---
**info**

查看各种信息：

info file  查看当前文件的信息，例如程序入口点（Entry point）

info break 查看当前断点信息

**disassemble+func** 对制定的函数进行反汇编

**break +&#8221;地址&#8221;** 设置断点

**r**  等同于“run” 运行程序

**c** 等同于&#8221;continue&#8221;,继续执行

<div>
  <b>x /<n/f/u> <addr></b>
</div>

<div>
  n、f、u是可选的参数。
</div>

<div>
  　　n是一个正整数，表示需要显示的内存单元的个数，也就是说从当前地址向后显示几个内存单元的内容，一个内存单元的大小由后面的u定义。
</div>

<div>
  　　f 表示显示的格式，参见下面。如果地址所指的是字符串，那么格式可以是s，如果地址是指令地址，那么格式可以是i。
</div>

<div>
  　　u 表示从当前地址往后请求的字节数，如果不指定的话，GDB默认是4个bytes。u参数可以用下面的字符来代替，b表示单字节，h表示双字节，w表示四字 节，g表示八字节。当我们指定了字节长度后，GDB会从指内存定的内存地址开始，读写指定字节，并把其当作一个值取出来。
</div>

<div>
  　　<addr>表示一个内存地址。
</div>

<div>
  　　注意：严格区分n和u的关系，n表示单元个数，u表示每个单元的大小。
</div>

<div>
  <div>
    <b>layout</b>：用于分割窗口，可以一边查看代码，一边测试。主要有以下几种用法：
  </div>
  
  <div>
    layout src：显示源代码窗口
  </div>
  
  <div>
    layout asm：显示汇编窗口
  </div>
  
  <div>
    layout regs：显示源代码/汇编和寄存器窗口
  </div>
  
  <div>
    layout split：显示源代码和汇编窗口
  </div>
  
  <div>
    layout next：显示下一个layout
  </div>
  
  <div>
    layout prev：显示上一个layout
  </div>
  
  <div>
    Ctrl + L：刷新窗口
  </div>
  
  <div>
    Ctrl + x，再按1：单窗口模式，显示一个窗口
  </div>
  
  <div>
    Ctrl + x，再按2：双窗口模式，显示两个窗口
  </div>
  
  <div>
    Ctrl + x，再按a：回到传统模式，即退出layout，回到执行layout之前的调试窗口。
  </div>
</div>

&nbsp;