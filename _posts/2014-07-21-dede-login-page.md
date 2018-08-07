---
id: 159
title: dedecms 找后台方法整理
date: 2014-07-21T10:55:29+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=159
permalink: /dede-login-page/
categories:
  - WEB
tags:
  - dede
  - dedecms
  - 后台
  - 织梦
---
前言：以上方法整理自互联网，如有侵权，请联系我

update:2015/1/23

<img src="http://kowapp.u.qiniudn.com/dedecms.jpg" alt="" border="0" />

首先是默认后台地址：/dede

然后，比较懒的站长会改成/admin,/manage （可以用burpsuite进行目录枚举）

织梦dedecms利用文件查找后台的技巧：
  
**1、**/include/dialog/select_media.php?f=form1.murl

**2、**/include/dialog/select_soft.php

**3.**使用mysql_error 信息去试试
  
/data/mysql\_error\_trace.inc  —— 建议使用该方法屡试屡爽

**4.**/robots.txt
  
/data/admin/ver.txt

**5.**DedeCmsV5.1 FreeSP1访问
  
http://127.0.0.1/include/dialog/ … =/include/FCKeditor
  
可以跳转目录跳转到根目录的方法为：
  
http://127.0.0.1/include/dialog/ … h=/././././././././
  
而且DEDECMS在访问不存在的目录时会报错如访问
  
http://127.0.0.1/include/dialog/ … t0pst0pst0pst0pst0p
  
DedeCMS V5.3.1和最新的DedeCMS V5.5正式版，发现这两个版本已经进行了处理，而且只会列出目录和一些允许显示的文件，PHP是不能显示了，爆路径是一样通用的构造

http://127.0.0.1/include/dialog/ … =/include/FCKeditor

**6.**大招（转自<a href="http://zone.wooyun.org/content/13115" target="_blank">微尘</a>）

其实方法很简单自己偶然发现的:

通过百度的浏量统计找到后台地址
  
只要是流量统计网站都可以,其实不用百度,
  
（1）xxoo:弄一个你能看到流量统计的域名
  
（2）ooxx:去给管理员留言,吧你域名通过网站留言发给他说要加友情链接,他肯定会在后台看的,

（3）oxxo:等他看了你的留言,他肯定会打开你的网址,你去自己域名的流量哪里看看     &#8216;入口页面&#8217;   就能看到他网站的后台地址

**7.利用xss**

**这个不能单单算是找后台了，因为xss会x到管理的cookie和后台地址，或者是xss结合csrf可以直接拿shell，详细请见<a href="http://zone.wooyun.org/content/13138" target="_blank">wooyun</a>**

**8.google hack(8月23日更新 转自：<a href="http://bbs.blackbap.org/thread-6517-1-1.html" target="_blank">中国杀猪刀</a>)**

<table class="ke-zeroborder" cellspacing="0" cellpadding="0">
  <tr>
    <td id="postmessage_101885" class="t_f">
      在搜索引擎里面所有这个关键字，点开连接之后，再去掉这个关键字，他的后台就出来了<br /> 原理就是dedecms网站的后台都有这么一个广告，而且通常都是在网站后台后面<br /> 所以，只要有这个广告的网站，基本都能暴露后台了。</p> 
      
      <p>
        这是关键字<br /> <b>inurl:?dopost=showad site:xxoo.com</b>
      </p>
      
      <p>
        &nbsp;</td> </tr> </tbody> </table> 
        
        <p>
          <b>9.织梦某处设计缺陷导致后台地址泄露(转自：<a href="http://www.wooyun.org/bugs/wooyun-2014-076556" target="_blank">wooyun</a>)</b>
        </p>
        
        <p class="detail">
          DEDECMS对友情链接申请的LOGO地址没有进行严格的判断和过滤，导致可以提交PHP、ASP等后缀。<br /> 且后台查看友情链接时，如下图所示，图片仍是申请时提交的URL。
        </p>
        
        <p class="detail usemasaic">
          <a href="http://www.wooyun.org/upload/201409/182148026db1033c8016627d5b0e2b3c684fe01b.png" target="_blank"><img src="http://www.wooyun.org/upload/201409/182148026db1033c8016627d5b0e2b3c684fe01b.png" alt="QQ截图20140918214426.png" width="600" /></a>
        </p>
        
        <p class="detail">
          X.PHP 代码:
        </p>
        
        <pre class="brush:php; toolbar: true; auto-links: true;">&lt;?php
file_put_contents('x.txt',$_SERVER['HTTP_REFERER']);
header("Content-type:image/jpeg");
$img=imagecreatefromjpeg("x.jpg");
imagejpeg($img);
imagedestroy($img);
?&gt;</pre>
        
        <p>
          当访问该文件，获取$_SERVER[&#8216;HTTP_REFERER&#8217;]，并且写出文件到X.TXT。
        </p>
        
        <p class="detail usemasaic">
          <a href="http://www.wooyun.org/upload/201409/1821504466cf3f982a6d750d2c5116afcdc4d623.png" target="_blank"><img src="http://www.wooyun.org/upload/201409/1821504466cf3f982a6d750d2c5116afcdc4d623.png" alt="QQ截图20140918214426.png" width="600" /></a>
        </p>
        
        <p>
          其他方法：可以社工猜解，利用google hack :&#8221;site:xxoo.com inurl:login.php 织梦管理中心&#8221;
        </p>
        
        <p>
          如果还是日不下来，换个思路吧~~
        </p>
        
        <p>
          本日志会随时更新
        </p>
        
        <p>
          &nbsp;
        </p>