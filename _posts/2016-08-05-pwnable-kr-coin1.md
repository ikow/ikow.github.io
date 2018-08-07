---
id: 60
title: pwnable.kr-coin1
date: 2016-08-05T05:02:24+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=60
permalink: /pwnable-kr-coin1/
categories:
  - pwn
tags:
  - coin
  - payload
  - pwnable.kr
  - socket
  - 二分法
---
题目描述：

> Mommy, I wanna play a game!
  
> (if your network response time is too slow, try nc 0 9007 inside pwnable.kr server)
> 
> Running at : nc pwnable.kr 9007

运行连接后发现是个小游戏：

> &#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;
  
> &#8211; Shall we play a game? &#8211;
  
> &#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;
> 
> You have given some gold coins in your hand
  
> however, there is one counterfeit coin among them
  
> counterfeit coin looks exactly same as real coin
  
> however, its weight is different from real one
  
> real coin weighs 10, counterfeit coin weighes 9
  
> help me to find the counterfeit coin with a scale
  
> if you find 100 counterfeit coins, you will get reward 🙂
  
> FYI, you have 30 seconds.
> 
> &#8211; How to play &#8211;
  
> 1. you get a number of coins (N) and number of chances (C)
  
> 2. then you specify a set of index numbers of coins to be weighed
  
> 3. you get the weight information
  
> 4. 2~3 repeats C time, then you give the answer
> 
> &#8211; Example &#8211;
  
> [Server] N=4 C=2 # find counterfeit among 4 coins with 2 trial
  
> [Client] 0 1 # weigh first and second coin
  
> [Server] 20 # scale result : 20
  
> [Client] 3 # weigh fourth coin
  
> [Server] 10 # scale result : 10
  
> [Client] 2 # counterfeit coin is third!
  
> [Server] Correct!
> 
> &#8211; Ready? starting in 3 sec&#8230; &#8211;

需要在C步内找到N个coin中那个假的coin，我们发现N<=2^C,所以直接使用二分法就好了，由于本地跑程序的话延迟太高，无法在30秒内跑完程序，所以我们使用之前其他题目的ssh，在tmp目录下运行我们的程序，最后的脚本为：

<pre class="brush: python; title: ; notranslate" title="">import socket
import re

HOST = '0.0.0.0'
PORT = 9007
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((HOST,PORT))
data = s.recv(2048)
#print data
while 1:
 data = s.recv(1024)
 #print data
 if(data.find("=")):
 N = re.findall(r'(\w*[0-9]+)\w*',data)
 a = int(N[0])
 b = int(N[1])
 start = 0
 end = a
 while(1):
 half = start + (end - start)/2 + (end - start)%2
 string = ""
 for i in range(start,half):
 string += str(i) + ' '
 string = string + "\n"
 #print string
 s.send(string)
 data = s.recv(1024)
 if(data.find("Correct!") == 0):
 print data
 break
 Rev_Num = int(data)
 #print start,"|",half,"|",end
 Sum = (half -start)*10
 #print Rev_Num, Sum
 if( Rev_Num &lt; Sum):
 end = half
 else:
 start = half

s.close()
</pre>

成功找到100个coin后，服务器返回flag：

b1NaRy\_S34rch1nG\_1s\_3asy\_p3