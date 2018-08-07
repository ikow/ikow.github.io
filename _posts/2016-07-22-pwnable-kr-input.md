---
id: 50
title: pwnable.kr-input
date: 2016-07-22T09:13:03+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=50
permalink: /pwnable-kr-input/
categories:
  - pwn
tags:
  - exploit
  - fd
  - fork
  - pipe
  - pwn
  - pwnable.kr
  - read()
  - shellcode
  - socket
  - 管道传输
  - 重定向
---
题目描述：

> Mom? how can I pass my input to a computer program?
> 
> ssh input@pwnable.kr -p2222 (pw:guest)

连接上ssh后，input.c的源码如下：

<pre class="brush: cpp; title: ; notranslate" title="">#include &lt;stdio.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;string.h&gt;
#include &lt;sys/socket.h&gt;
#include &lt;arpa/inet.h&gt;

int main(int argc, char* argv[], char* envp[]){
 printf("Welcome to pwnable.kr\n");
 printf("Let's see if you know how to give input to program\n");
 printf("Just give me correct inputs then you will get the flag :)\n");

 // argv
 if(argc != 100) return 0;
 if(strcmp(argv['A'],"\x00")) return 0;
 if(strcmp(argv['B'],"\x20\x0a\x0d")) return 0;
 printf("Stage 1 clear!\n");

 // stdio
 char buf[4];
 read(0, buf, 4);
 if(memcmp(buf, "\x00\x0a\x00\xff", 4)) return 0;
 read(2, buf, 4);
 if(memcmp(buf, "\x00\x0a\x02\xff", 4)) return 0;
 printf("Stage 2 clear!\n");

 // env
 if(strcmp("\xca\xfe\xba\xbe", getenv("\xde\xad\xbe\xef"))) return 0;
 printf("Stage 3 clear!\n");

 // file
 FILE* fp = fopen("\x0a", "r");
 if(!fp) return 0;
 if( fread(buf, 4, 1, fp)!=1 ) return 0;
 if( memcmp(buf, "\x00\x00\x00\x00", 4) ) return 0;
 fclose(fp);
 printf("Stage 4 clear!\n");

 // network
 int sd, cd;
 struct sockaddr_in saddr, caddr;
 sd = socket(AF_INET, SOCK_STREAM, 0);
 if(sd == -1){
 printf("socket error, tell admin\n");
 return 0;
 }
 saddr.sin_family = AF_INET;
 saddr.sin_addr.s_addr = INADDR_ANY;
 saddr.sin_port = htons( atoi(argv['C']) );
 if(bind(sd, (struct sockaddr*)&amp;saddr, sizeof(saddr)) &lt; 0){
 printf("bind error, use another port\n");
 return 1;
 }
 listen(sd, 1);
 int c = sizeof(struct sockaddr_in);
 cd = accept(sd, (struct sockaddr *)&amp;caddr, (socklen_t*)&amp;c);
 if(cd &lt; 0){
 printf("accept error, tell admin\n");
 return 0;
 }
 if( recv(cd, buf, 4, 0) != 4 ) return 0;
 if(memcmp(buf, "\xde\xad\xbe\xef", 4)) return 0;
 printf("Stage 5 clear!\n");

 // here's your flag
 system("/bin/cat flag");
 return 0;
}
</pre>

这题代码显而易见并没有需要溢出的地方，这里考察的是unix下的数据传输，我们分别看下5个stage。

**Stage1** 

需要通过argv传递参数，由于这里需要满足argv[&#8216;A&#8217;]=&#8221;\x00&#8243; argv[&#8216;B&#8217;]=&#8221;\x20\x0a\x0d&#8221;，这些都是不可见字符，并且是空字符以及回车换行符，所以这里不能够使用python通过命令行传递参数，这里使用C语言编写程序，通过子进程的方法调用input程序进行传参

**Stage2**

这里我们又遇到read函数了，在pwnable.kr的第一题中就涉及到：[pwnable.kr-fd](http://www.ikow.cn/pwnable-kr-fd/)

当fd为0时，为stdin ，fd为1时，为stdout，fd为2时，为stderr，

所以这里我们使用fork创建子进程，pipe进行管道传输，具体见下面的代码

**Stage3**

stage3使用env传递参数，env是标准main函数三个参数中的一个，它的值是系统环境变量，默认是不需要填写的。这里我们需要通过设定env=&#8221;\xde\xad\xbe\xef=\xca\xfe\xba\xbe&#8221; ，然后通过execve(&#8220;/root/Desktop/input&#8221;, argv, env);进行传递

**Stage4**

需要在文件名为”\x0a“的文件中读取字符，判断是否为&#8221;\x00\x00\x00\x00&#8243;，这里直接把源代码中的读取文件改成写文件就可以了：

<pre class="brush: cpp; title: ; notranslate" title="">FILE* fp = fopen("\x0a", "w");
 if(!fp){printf("cannot open\n");return 0;}
 char *buff = "\x00\x00\x00\x00";
 fwrite(buff, 4, 1, fp);
 fclose(fp);
</pre>

**Stage5**

socket编程，同样和4一样，只要把接受改成发送就可以了，这里需要注意的是socket的端口是通过argv[&#8216;C&#8217;]来控制的，所以我们需要设置好argv[&#8216;C&#8217;]，然后在执行第五部之前需要有一个延时，因为这里我们需要时间完成第四部不同进程间的通信：

<pre class="brush: cpp; title: ; notranslate" title="">sleep(5);
 int sd;
 struct sockaddr_in saddr;
 sd = socket(AF_INET, SOCK_STREAM, 0);
 if(sd == -1)
 {
 printf("socket error, tell admin\n");
 return 0;
 }
 saddr.sin_family = AF_INET;
 saddr.sin_addr.s_addr = inet_addr("127.0.0.1");
 saddr.sin_port = htons(2333);
 if(connect(sd, (struct sockaddr*) &amp;saddr, sizeof(saddr)))
 {
 perror("Problem connecting\n");
 exit(1);
 }
 printf("Connected\n");
 write(sd,"\xde\xad\xbe\xef",4);
 close(sd);
</pre>

所以最后的代码为：

<pre class="brush: cpp; title: ; notranslate" title="">#include&lt;unistd.h&gt;
#include&lt;string.h&gt;
#include&lt;stdio.h&gt;
#include&lt;sys/socket.h&gt;
#include&lt;arpa/inet.h&gt;
#include&lt;stdlib.h&gt;

int main()
{
 int pipe0[2], pipe1[2];
 char *argv[101] = {[0 ... 99] = "A"};
 argv['A'] = "\x00";
 argv['B'] = "\x20\x0a\x0d";
 argv['C'] = "55555";
 char *env[2] = {"\xde\xad\xbe\xef=\xca\xfe\xba\xbe"};

 FILE* fp = fopen("\x0a", "w");
 if(!fp){printf("cannot open\n");return 0;}
 char *buff = "\x00\x00\x00\x00";
 fwrite(buff, 4, 1, fp);
 fclose(fp);

 if(pipe(pipe0) &lt; 0 || pipe(pipe1) &lt; 0)
 {
 printf("pipe create error\n");
 return 0;
 }
 if(fork() == 0)
 {
 dup2(pipe0[0],0);
 dup2(pipe1[0],2);
 close(pipe0[1]);
 close(pipe1[1]);
 execve("/root/Desktop/input", argv, env);
 }
 else
 {
 write(pipe0[1],"\x00\x0a\x00\xff",4);
 write(pipe1[1],"\x00\x0a\x02\xff",4);
 close(pipe0[1]);
 close(pipe1[1]);

 }
 //network
 sleep(5);
 int sd;
 struct sockaddr_in saddr;
 sd = socket(AF_INET, SOCK_STREAM, 0);
 if(sd == -1)
 {
 printf("socket error, tell admin\n");
 return 0;
 }
 saddr.sin_family = AF_INET;
 saddr.sin_addr.s_addr = inet_addr("127.0.0.1");
 saddr.sin_port = htons(55555);
 if(connect(sd, (struct sockaddr*) &saddr, sizeof(saddr)))
 {
 perror("Problem connecting\n");
 exit(1);
 }
 printf("Connected\n");
 write(sd,"\xde\xad\xbe\xef",4);
 close(sd);
 return 0;
}
</pre>

这里不要忘了添加文件头，然后我们把代码放到pwnable.kr服务器上的/tmp/input目录，由于目录下并没有flag，我们执行ln /home/input/flag flag，把flag重定向到当前目录下，最后编译运行得到flag：

> Just give me correct inputs then you will get the flag 🙂
  
> Stage 1 clear!
  
> Stage 2 clear!
  
> Stage 3 clear!
  
> Stage 4 clear!
  
> Connected
  
> Stage 5 clear!
  
> Mommy! I learned how to pass various input in Linux 🙂

flag为：Mommy! I learned how to pass various input in Linux 🙂