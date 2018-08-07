---
id: 62
title: pwnable.kr-blackjack
date: 2016-08-09T06:56:06+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=62
permalink: /pwnable-kr-blackjack/
categories:
  - pwn
tags:
  - 21点
  - blackjack
  - bypass
  - pwnable.kr
---
题目描述：

> Hey! check out this C implementation of blackjack game!
  
> I found it online
  
> * http://cboard.cprogramming.com/c-programming/114023-simple-blackjack-program.html
> 
> I like to give my flags to millionares.
  
> how much money you got?
  
> Running at : nc pwnable.kr 9009

看了下源码，就是个blackjack（21点）游戏，本还以为是需要自己写个机器人，过关的要求的是millionaire（100万），结果黑盒就过了，过程很简单，在投注的时候输了一个很大的数，第一次没有通过，要求重新输入，然后再输一次，并赢了这局就可以了。

看下源码，很容易发现有问题的地方：

<pre class="brush: cpp; title: ; notranslate" title="">int betting() //Asks user amount to bet
{
 printf("\n\nEnter Bet: $");
 scanf("%d", &bet);
 
 if (bet &gt; cash) //If player tries to bet more money than player has
 {
        printf("\nYou cannot bet more money than you have.");
        printf("\nEnter Bet: ");
        scanf("%d", &bet);
        return bet;
 }
 else return bet;
} // End Function
</pre>

这里if应该改成while，如果是if的话，这里只做了一次验证，第二次输入的bet并没有验证：

这样成功获得了flag：

> YaY\_I\_AM\_A\_MILLIONARE_LOL
  
> Cash: $727380468
  
> &#8212;&#8212;-
  
> |S |
  
> | 9 |
  
> | S|
  
> &#8212;&#8212;-
> 
> Your Total is 9
> 
> The Dealer Has a Total of 10

flag是：

YaY\_I\_AM\_A\_MILLIONARE_LOL