---
id: 147
title: OJ解决Presentation Error
date: 2016-02-27T14:22:55+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=147
permalink: /presentation-error/
categories:
  - 未分类
tags:
  - Error
  - OJ
  - Presentation
  - program
  - UVa
  - 编程
---
<img src="https://valleytechnologies.net/wp-content/uploads/2015/07/error.png" alt="" border="0" />

在练习UVa1225的时候，提交如下代码，最终结果为Presentation Error:

<pre class="brush:cpp; toolbar: true; auto-links: true;">#include&lt;stdio.h&gt;
#include&lt;string.h&gt;
int main()
{
    int n;
    int T;
    scanf("%d", &T);
    while(T--){
    int a[10];
    memset(a, 0, sizeof(a));
    scanf("%d", &n);
    for(int i = 0; i &lt;= n; i ++)
    {
        int j;
        j = i;
        while(j)
        {
            a[j%10] ++;
            j /= 10;
        }
    }
    for(int i = 0; i &lt; 10; i++)
        printf("%d ", a[i]);
    printf("\n");
    }
    return 0;
}</pre>

平时上课总是做presentation，现在出了presentation error应该是结果显示的时候出了问题，于是修改代码如下：

<pre class="brush:cpp; toolbar: true; auto-links: true;">#include&lt;stdio.h&gt;
#include&lt;string.h&gt;
int main()
{
    int n;
    int T;
    scanf("%d", &T);
    while(T--){
    int a[10];
    memset(a, 0, sizeof(a));
    scanf("%d", &n);
    for(int i = 0; i &lt;= n; i ++)
    {
        int j;
        j = i;
        while(j)
        {
            a[j%10] ++;
            j /= 10;
        }
    }
    for(int i = 0; i &lt; 9; i++)
        printf("%d ", a[i]);
    printf("%d\n", a[9]);
    }
    return 0;
}</pre>

提交后，成功AC了。

总结下，遇到Presentation Error是输出出现了问题，修改下输出结果，注意空格或者制表符。