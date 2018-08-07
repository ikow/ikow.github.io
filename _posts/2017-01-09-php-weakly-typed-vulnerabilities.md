---
id: 4433
title: PHP弱类型漏洞总结
date: 2017-01-09T04:07:05+00:00
author: admin
layout: post
guid: http://blog.ikow.cn/?p=4433
permalink: /php-weakly-typed-vulnerabilities/
categories:
  - WEB
tags:
  - md5
  - PHP
  - weakly typed
  - 弱类型
  - 弱类型漏洞
---
首先说下强弱类型，根据维基百科的定义：

> In computer programming, programming languages are often colloquially classified as **strongly typed** or **weakly typed** (**loosely typed**). These terms do not have a precise definition, but in general, a strongly typed language is more likely to generate an error or refuse to compile if the argument passed to a function does not closely match the expected type. On the other hand, a very weakly typed language may produce unpredictable results or may perform implicit type conversion.

对于强弱类型，并没有明确的定义，但是可以通过函数传参进行判断：在某个编程语言里面，给某个函数传了一个不是所需类型的参数，如果编译器报错了，那么可以认为它是强类型语言(strongly typed)，反之则认为是弱类型语言(weakly typed)。

这里我们先不直接下结论说PHP是什么类型的（当然大家都知道了），我们按照维基百科的标准来测试一下：

<pre class="brush: php; title: ; notranslate" title="">&lt;?php $a = "string"; function test($b) { $b = $b + 1; echo $b; } test($a); ?&gt;
</pre>

最后的结果是在屏幕上输出1，并没有报错，所以PHP是弱类型语言。

弱类型语言在使用上更为方便快捷，但是同样也带来了一些弊端，也就是这篇文章要总结的PHP弱类型导致的漏洞：

**1、使用松散比较“==”**

(1)$a=null;$b=flase ;

(2)$a=&#8221;;$b=null;

(3)$a=&#8217;0&#8242;;$b=0;

(4)$a=&#8217;abcdefg&#8217;;$b=0;

(5)$a=&#8217;1abcdefg&#8217;;$b=1;

(6)$a=&#8217;0e391845&#8242;;$b=&#8217;0e1948284&#8242;;

(7)$a=&#8217;0x1240&#8242;;$b=123456;

以上这些当我们用$a==$b进行比较时，都会返回true,其中(6)中例如0e\b+这种形式的都会认为是0，所以返回true，这点特性在后面md5()中也会体现，(7)中0x\b+ PHP会认为是16进制，等于10进制的123456.

&nbsp;

**2、switch() 函数自动转换**

在使用switch函数时，switch会把传入的参数经过intval处理，这样例如“0euqie&#8221;就会变成0，“1eidue”就会变成1，这样会导致运行错误的条件结果

&nbsp;

3、md5()

md5函数在PHP中有两个问题，第一个问题我们前面提到型如0e\b+类型的变量都会认为相等，这样就导致了两个变量它们本身的值不相同，但是经过md5返回的哈希值都是0e\b+类型的，结果变成了“相等”的。例如在wargame.kr里面有一题：

<pre class="brush: php; title: ; notranslate" title="">&lt;?php
    if (isset($_GET['view-source'])) {
         show_source(__FILE__);
         exit();
    }

    if (isset($_GET['v1']) && isset($_GET['v2'])) {
        sleep(3); // anti brute force

        $chk = true;
        $v1 = $_GET['v1'];
        $v2 = $_GET['v2'];

        if (!ctype_alpha($v1)) {$chk = false;}
        if (!is_numeric($v2) ) {$chk = false;}
        if (md5($v1) != md5($v2)) {$chk = false;}

        if ($chk){
            include("../lib.php");
            echo "Congratulations! FLAG is : ".auth_code("md5_compare");
        } else {
            echo "Wrong...";
        }
    }
?&gt;
&lt;br /&gt;
&lt;form method="GET"&gt;
    VALUE 1 : &lt;input type="text" name="v1" /&gt;&lt;br /&gt;
    VALUE 2 : &lt;input type="text" name="v2" /&gt;&lt;br /&gt;
    &lt;input type="submit" value="chk" /&gt;
&lt;/form&gt;
&lt;br /&gt;
&lt;a href="?view-source"&gt;view-source&lt;/a&gt;
</pre>

经过测试，我们发现：

>         $ echo -n 240610708 | md5sum
>         0e462097431906509019562988736854  -
>         $ echo -n QNKCDZO | md5sum
>         0e830400451993494058024219903391  -
>         $ echo -n aabg7XSs | md5sum
>         0e087386482136013740957780965295  -

这三组最后生成的md5都是0e\b+形式的，所以我们最后构造v1=QNKCDZO 和 v2=240610708就能成功绕过判断

另外一个就是：PHP手册中的md5()函数的描述是string md5 ( string $str [, bool $raw_output = false ] )，md5()中的需要是一个string类型的参数。但是当你传递一个array时，md5()不会报错，知识会无法正确地求出array的md5值，这样就会导致任意2个array的md5值都会相等。

<pre class="brush: php; title: ; notranslate" title="">$array1[] = array(
"foo" =&gt; "bar",
"bar" =&gt; "foo",
);
$array2 = array("foo", "bar", "hello", "world");
var_dump(md5($array1)==var_dump($array2)); //true
</pre>

&nbsp;

4、in_array()

在PHP手册中，in\_array()函数的解释是bool in\_array ( mixed $needle , array $haystack [, bool $strict = FALSE ] ),如果strict参数没有提供，那么in\_array就会使用松散比较来判断$needle是否在$haystack中。当strince的值为true时，in\_array()会比较needls的类型和haystack中的类型是否相同。

<pre class="brush: php; title: ; notranslate" title="">$array=[0,1,2,'3'];
var_dump(in_array('abc', $array)); //true
var_dump(in_array('1bc', $array)); //true
</pre>

可以看到上面的情况返回的都是true,因为’abc’会转换为0，’1bc’转换为1。

array\_search()与in\_array()也是一样的问题