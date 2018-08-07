---
id: 113
title: 利用搜索引擎批量抓取url
date: 2014-12-25T09:58:11+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=113
permalink: /catch-url-from-search-engine/
categories:
  - WEB
tags:
  - fofa
  - google
  - search
  - shodan
  - url
  - 搜索引擎
---
有的时候爆出0day，我们需要抓取大量的url进行测试，这个时候需要用到搜索引擎，常用的有：google，fofa，shodan
  
下面是整理的利用js对对不同搜索引擎进行批量抓取的代码：
  
fofa: （如何使用：StartReq(搜索语法,开始页码,结束页码) ）

<ol class="linenums">
  <li class="L0">
    <code>&lt;span class="typ">StartReq&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">'body=wooyun'&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="lit">1&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="lit">10&lt;/span>&lt;span class="pun">)&lt;/span> </code>
  </li>
  <li class="L1">
    <code>&lt;span class="kwd">function&lt;/span> &lt;span class="typ">StartReq&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">q&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln">startpage&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln">endpage&lt;/span>&lt;span class="pun">){&lt;/span> </code>
  </li>
  <li class="L2">
    <code> &lt;span class="kwd">for&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="kwd">var&lt;/span>&lt;span class="pln"> i&lt;/span>&lt;span class="pun">=&lt;/span>&lt;span class="pln">startpage&lt;/span>&lt;span class="pun">;&lt;/span>&lt;span class="pln">i&lt;/span>&lt;span class="pun">&lt;=&lt;/span>&lt;span class="pln">endpage&lt;/span>&lt;span class="pun">;&lt;/span>&lt;span class="pln">i&lt;/span>&lt;span class="pun">++){&lt;/span> </code>
  </li>
  <li class="L3">
    <code> &lt;span class="typ">Req&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">i&lt;/span>&lt;span class="pun">+&lt;/span>&lt;span class="str">"q="&lt;/span>&lt;span class="pun">+&lt;/span>&lt;span class="pln">encodeURIComponent&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">q&lt;/span>&lt;span class="pun">)+&lt;/span>&lt;span class="str">"&qbase64="&lt;/span>&lt;span class="pun">+&lt;/span>&lt;span class="pln">btoa&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">q&lt;/span>&lt;span class="pun">));&lt;/span> </code>
  </li>
  <li class="L4">
    <code> &lt;span class="pun">}&lt;/span> </code>
  </li>
  <li class="L5">
    <code>&lt;span class="pun">}&lt;/span> </code>
  </li>
  <li class="L6">
    <code>&lt;span class="kwd">function&lt;/span> &lt;span class="typ">Connection&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="typ">Sendtype&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln">url&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln">content&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln">callback&lt;/span>&lt;span class="pun">){&lt;/span> </code>
  </li>
  <li class="L7">
    <code> &lt;span class="kwd">if&lt;/span> &lt;span class="pun">(&lt;/span>&lt;span class="pln">window&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="typ">XMLHttpRequest&lt;/span>&lt;span class="pun">){&lt;/span> </code>
  </li>
  <li class="L8">
    <code> &lt;span class="kwd">var&lt;/span>&lt;span class="pln"> xmlhttp&lt;/span>&lt;span class="pun">=&lt;/span>&lt;span class="kwd">new&lt;/span> &lt;span class="typ">XMLHttpRequest&lt;/span>&lt;span class="pun">();&lt;/span> </code>
  </li>
  <li class="L9">
    <code> &lt;span class="pun">}&lt;/span> </code>
  </li>
  <li class="L0">
    <code> &lt;span class="kwd">else&lt;/span>&lt;span class="pun">{&lt;/span> </code>
  </li>
  <li class="L1">
    <code> &lt;span class="kwd">var&lt;/span>&lt;span class="pln"> xmlhttp&lt;/span>&lt;span class="pun">=&lt;/span>&lt;span class="kwd">new&lt;/span> &lt;span class="typ">ActiveXObject&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">"Microsoft.XMLHTTP"&lt;/span>&lt;span class="pun">);&lt;/span> </code>
  </li>
  <li class="L2">
    <code> &lt;span class="pun">}&lt;/span> </code>
  </li>
  <li class="L3">
    <code>&lt;span class="pln"> xmlhttp&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">onreadystatechange&lt;/span>&lt;span class="pun">=&lt;/span>&lt;span class="kwd">function&lt;/span>&lt;span class="pun">(){&lt;/span> </code>
  </li>
  <li class="L4">
    <code> &lt;span class="kwd">if&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">xmlhttp&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">readyState&lt;/span>&lt;span class="pun">==&lt;/span>&lt;span class="lit">4&lt;/span>&lt;span class="pun">&&&lt;/span>&lt;span class="pln">xmlhttp&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">status&lt;/span>&lt;span class="pun">==&lt;/span>&lt;span class="lit">200&lt;/span>&lt;span class="pun">)&lt;/span> </code>
  </li>
  <li class="L5">
    <code> &lt;span class="pun">{&lt;/span> </code>
  </li>
  <li class="L6">
    <code>&lt;span class="pln"> callback&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">xmlhttp&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">responseText&lt;/span>&lt;span class="pun">);&lt;/span> </code>
  </li>
  <li class="L7">
    <code> &lt;span class="pun">}&lt;/span> </code>
  </li>
  <li class="L8">
    <code> &lt;span class="pun">}&lt;/span> </code>
  </li>
  <li class="L9">
    <code>&lt;span class="pln"> xmlhttp&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">open&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="typ">Sendtype&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln">url&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="kwd">true&lt;/span>&lt;span class="pun">);&lt;/span> </code>
  </li>
  <li class="L0">
    <code>&lt;span class="pln"> xmlhttp&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">setRequestHeader&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">"Content-Type"&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="str">"application/x-www-form-urlencoded"&lt;/span>&lt;span class="pun">);&lt;/span> </code>
  </li>
  <li class="L1">
    <code>&lt;span class="pln"> xmlhttp&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">send&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">content&lt;/span>&lt;span class="pun">);&lt;/span> </code>
  </li>
  <li class="L2">
    <code>&lt;span class="pun">}&lt;/span> </code>
  </li>
  <li class="L3">
    <code>&lt;span class="kwd">function&lt;/span> &lt;span class="typ">Req&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">searchString&lt;/span>&lt;span class="pun">){&lt;/span> </code>
  </li>
  <li class="L4">
    <code> &lt;span class="kwd">var&lt;/span>&lt;span class="pln"> searchurl &lt;/span>&lt;span class="pun">=&lt;/span> &lt;span class="str">"http://fofa.so/search/result?page="&lt;/span>&lt;span class="pun">+&lt;/span>&lt;span class="pln">searchString&lt;/span>&lt;span class="pun">;&lt;/span> </code>
  </li>
  <li class="L5">
    <code> &lt;span class="typ">Connection&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">"GET"&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln">searchurl&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="str">""&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="kwd">function&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">callback&lt;/span>&lt;span class="pun">){&lt;/span> </code>
  </li>
  <li class="L6">
    <code> &lt;span class="kwd">var&lt;/span>&lt;span class="pln"> result &lt;/span>&lt;span class="pun">=&lt;/span>&lt;span class="pln"> $&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">callback&lt;/span>&lt;span class="pun">);&lt;/span> </code>
  </li>
  <li class="L7">
    <code>&lt;span class="pln"> result&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">find&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">'div.col-lg-4 a'&lt;/span>&lt;span class="pun">).&lt;/span>&lt;span class="pln">each&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="kwd">function&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">i&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln">o&lt;/span>&lt;span class="pun">){&lt;/span> </code>
  </li>
  <li class="L8">
    <code> &lt;span class="kwd">var&lt;/span>&lt;span class="pln"> o &lt;/span>&lt;span class="pun">=&lt;/span>&lt;span class="pln"> $&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">o&lt;/span>&lt;span class="pun">);&lt;/span> </code>
  </li>
  <li class="L9">
    <code> &lt;span class="kwd">if&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">o&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">attr&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">'target'&lt;/span>&lt;span class="pun">)==&lt;/span>&lt;span class="str">"_blank"&lt;/span>&lt;span class="pun">){&lt;/span> </code>
  </li>
  <li class="L0">
    <code> &lt;span class="kwd">if&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">o&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">attr&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">'href'&lt;/span>&lt;span class="pun">).&lt;/span>&lt;span class="pln">indexOf&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">'/search/checkapp?all=true&host='&lt;/span>&lt;span class="pun">)){&lt;/span> </code>
  </li>
  <li class="L1">
    <code>&lt;span class="pln"> console&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">log&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">o&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">attr&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">'href'&lt;/span>&lt;span class="pun">));&lt;/span> </code>
  </li>
  <li class="L2">
    <code> &lt;span class="pun">}&lt;/span> </code>
  </li>
  <li class="L3">
    <code> &lt;span class="pun">}&lt;/span> </code>
  </li>
  <li class="L4">
    <code> &lt;span class="pun">})&lt;/span> </code>
  </li>
  <li class="L5">
    <code> &lt;span class="pun">})&lt;/span> </code>
  </li>
  <li class="L6">
    <code> &lt;span class="pun">}&lt;/span></code>
  </li>
</ol>

google:

<ol class="linenums">
  <li class="L0">
    <code>&lt;span class="typ">StartReq&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">"site:xss1.com"&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="lit">1&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="lit">1&lt;/span>&lt;span class="pun">);&lt;/span> </code>
  </li>
  <li class="L1">
    <code>&lt;span class="kwd">var&lt;/span>&lt;span class="pln"> tmp &lt;/span>&lt;span class="pun">=&lt;/span> &lt;span class="pun">[];&lt;/span> </code>
  </li>
  <li class="L2">
    <code>&lt;span class="kwd">var&lt;/span> &lt;span class="typ">HerfRegExp&lt;/span> &lt;span class="pun">=&lt;/span> &lt;span class="str">/http:\/\/\w.*\/|https:\/\/\w.*\//&lt;/span>&lt;span class="pun">;&lt;/span> </code>
  </li>
  <li class="L3">
    <code>&lt;span class="pln">document&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">body&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">appendChild&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">document&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">createElement&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">'script'&lt;/span>&lt;span class="pun">)).&lt;/span>&lt;span class="pln">src&lt;/span>&lt;span class="pun">=&lt;/span>&lt;span class="str">'//code.jquery.com/jquery-1.9.1.min.js'&lt;/span>&lt;span class="pun">;&lt;/span> </code>
  </li>
  <li class="L4">
    <code>&lt;span class="kwd">function&lt;/span> &lt;span class="typ">StartReq&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">q&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln">startpage&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln">endpage&lt;/span>&lt;span class="pun">){&lt;/span> </code>
  </li>
  <li class="L5">
    <code> &lt;span class="kwd">for&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="kwd">var&lt;/span>&lt;span class="pln"> i&lt;/span>&lt;span class="pun">=&lt;/span>&lt;span class="pln">startpage&lt;/span>&lt;span class="pun">;&lt;/span>&lt;span class="pln">i&lt;/span>&lt;span class="pun">&lt;=&lt;/span>&lt;span class="pln">endpage&lt;/span>&lt;span class="pun">;&lt;/span>&lt;span class="pln">i&lt;/span>&lt;span class="pun">++){&lt;/span> </code>
  </li>
  <li class="L6">
    <code> &lt;span class="kwd">if&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">i&lt;/span>&lt;span class="pun">==&lt;/span>&lt;span class="lit">1&lt;/span>&lt;span class="pun">){&lt;/span> </code>
  </li>
  <li class="L7">
    <code> &lt;span class="typ">Req&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">"q="&lt;/span>&lt;span class="pun">+&lt;/span>&lt;span class="pln">encodeURIComponent&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">q&lt;/span>&lt;span class="pun">)+&lt;/span>&lt;span class="str">"&start=100&num=100&newwindow="&lt;/span>&lt;span class="pun">+&lt;/span>&lt;span class="pln">i&lt;/span>&lt;span class="pun">);&lt;/span> </code>
  </li>
  <li class="L8">
    <code> &lt;span class="pun">}&lt;/span> </code>
  </li>
  <li class="L9">
    <code> &lt;span class="kwd">else&lt;/span>&lt;span class="pun">{&lt;/span> </code>
  </li>
  <li class="L0">
    <code> &lt;span class="typ">Req&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">"q="&lt;/span>&lt;span class="pun">+&lt;/span>&lt;span class="pln">encodeURIComponent&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">q&lt;/span>&lt;span class="pun">)+&lt;/span>&lt;span class="str">"&start="&lt;/span>&lt;span class="pun">+(&lt;/span>&lt;span class="pln">i&lt;/span>&lt;span class="pun">*&lt;/span>&lt;span class="lit">100&lt;/span>&lt;span class="pun">)+&lt;/span>&lt;span class="str">"&num=100&newwindow="&lt;/span>&lt;span class="pun">+&lt;/span>&lt;span class="pln">i&lt;/span>&lt;span class="pun">);&lt;/span> </code>
  </li>
  <li class="L1">
    <code> &lt;span class="pun">}&lt;/span> </code>
  </li>
  <li class="L2">
    <code> &lt;span class="pun">}&lt;/span> </code>
  </li>
  <li class="L3">
    <code>&lt;span class="pun">}&lt;/span> </code>
  </li>
  <li class="L4">
    <code>&lt;span class="kwd">function&lt;/span> &lt;span class="typ">Connection&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="typ">Sendtype&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln">url&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln">content&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln">callback&lt;/span>&lt;span class="pun">){&lt;/span> </code>
  </li>
  <li class="L5">
    <code> &lt;span class="kwd">if&lt;/span> &lt;span class="pun">(&lt;/span>&lt;span class="pln">window&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="typ">XMLHttpRequest&lt;/span>&lt;span class="pun">){&lt;/span> </code>
  </li>
  <li class="L6">
    <code> &lt;span class="kwd">var&lt;/span>&lt;span class="pln"> xmlhttp&lt;/span>&lt;span class="pun">=&lt;/span>&lt;span class="kwd">new&lt;/span> &lt;span class="typ">XMLHttpRequest&lt;/span>&lt;span class="pun">();&lt;/span> </code>
  </li>
  <li class="L7">
    <code> &lt;span class="pun">}&lt;/span> </code>
  </li>
  <li class="L8">
    <code> &lt;span class="kwd">else&lt;/span>&lt;span class="pun">{&lt;/span> </code>
  </li>
  <li class="L9">
    <code> &lt;span class="kwd">var&lt;/span>&lt;span class="pln"> xmlhttp&lt;/span>&lt;span class="pun">=&lt;/span>&lt;span class="kwd">new&lt;/span> &lt;span class="typ">ActiveXObject&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">"Microsoft.XMLHTTP"&lt;/span>&lt;span class="pun">);&lt;/span> </code>
  </li>
  <li class="L0">
    <code> &lt;span class="pun">}&lt;/span> </code>
  </li>
  <li class="L1">
    <code>&lt;span class="pln"> xmlhttp&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">onreadystatechange&lt;/span>&lt;span class="pun">=&lt;/span>&lt;span class="kwd">function&lt;/span>&lt;span class="pun">(){&lt;/span> </code>
  </li>
  <li class="L2">
    <code> &lt;span class="kwd">if&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">xmlhttp&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">readyState&lt;/span>&lt;span class="pun">==&lt;/span>&lt;span class="lit">4&lt;/span>&lt;span class="pun">&&&lt;/span>&lt;span class="pln">xmlhttp&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">status&lt;/span>&lt;span class="pun">==&lt;/span>&lt;span class="lit">200&lt;/span>&lt;span class="pun">)&lt;/span> </code>
  </li>
  <li class="L3">
    <code> &lt;span class="pun">{&lt;/span> </code>
  </li>
  <li class="L4">
    <code>&lt;span class="pln"> callback&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">xmlhttp&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">responseText&lt;/span>&lt;span class="pun">);&lt;/span> </code>
  </li>
  <li class="L5">
    <code> &lt;span class="pun">}&lt;/span> </code>
  </li>
  <li class="L6">
    <code> &lt;span class="pun">}&lt;/span> </code>
  </li>
  <li class="L7">
    <code>&lt;span class="pln"> xmlhttp&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">open&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="typ">Sendtype&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln">url&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="kwd">true&lt;/span>&lt;span class="pun">);&lt;/span> </code>
  </li>
  <li class="L8">
    <code>&lt;span class="pln"> xmlhttp&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">setRequestHeader&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">"Content-Type"&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="str">"application/x-www-form-urlencoded"&lt;/span>&lt;span class="pun">);&lt;/span> </code>
  </li>
  <li class="L9">
    <code>&lt;span class="pln"> xmlhttp&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">send&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">content&lt;/span>&lt;span class="pun">);&lt;/span> </code>
  </li>
  <li class="L0">
    <code>&lt;span class="pun">}&lt;/span> </code>
  </li>
  <li class="L1">
    <code>&lt;span class="kwd">function&lt;/span> &lt;span class="typ">Req&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">searchString&lt;/span>&lt;span class="pun">){&lt;/span> </code>
  </li>
  <li class="L2">
    <code> &lt;span class="kwd">var&lt;/span>&lt;span class="pln"> searchurl &lt;/span>&lt;span class="pun">=&lt;/span> &lt;span class="str">"https://www.google.com.hk/search?"&lt;/span>&lt;span class="pun">+&lt;/span>&lt;span class="pln">searchString&lt;/span>&lt;span class="pun">;&lt;/span> </code>
  </li>
  <li class="L3">
    <code> &lt;span class="typ">Connection&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">"GET"&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln">searchurl&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="str">""&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="kwd">function&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">callback&lt;/span>&lt;span class="pun">){&lt;/span> </code>
  </li>
  <li class="L4">
    <code> &lt;span class="kwd">var&lt;/span>&lt;span class="pln"> result &lt;/span>&lt;span class="pun">=&lt;/span>&lt;span class="pln"> $&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">callback&lt;/span>&lt;span class="pun">);&lt;/span> </code>
  </li>
  <li class="L5">
    <code>&lt;span class="pln"> result&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">find&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">'div.rc h3.r a'&lt;/span>&lt;span class="pun">).&lt;/span>&lt;span class="pln">each&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="kwd">function&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">i&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln">o&lt;/span>&lt;span class="pun">){&lt;/span> </code>
  </li>
  <li class="L6">
    <code> &lt;span class="kwd">var&lt;/span>&lt;span class="pln"> o &lt;/span>&lt;span class="pun">=&lt;/span>&lt;span class="pln"> $&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">o&lt;/span>&lt;span class="pun">);&lt;/span> </code>
  </li>
  <li class="L7">
    <code>&lt;span class="pln"> tmp&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">push&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="typ">String&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="typ">HerfRegExp&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="kwd">exec&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">o&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">attr&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">'href'&lt;/span>&lt;span class="pun">))));&lt;/span> </code>
  </li>
  <li class="L8">
    <code> &lt;span class="pun">})&lt;/span> </code>
  </li>
  <li class="L9">
    <code> &lt;span class="pun">})&lt;/span> </code>
  </li>
  <li class="L0">
    <code>&lt;span class="pun">}&lt;/span></code>
  </li>
</ol>

最后的结果不会输出，会存入到tmp 数组，方便去重，如果需要输出可以自行加个循环tmp 把值打印出来
  
如何使用：StartReq(搜索语法,开始页码,结束页码)

shodan：

<ol class="linenums">
  <li class="L0">
    <code>&lt;span class="kwd">var&lt;/span>&lt;span class="pln"> url &lt;/span>&lt;span class="pun">=&lt;/span> &lt;span class="str">"http://www.shodanhq.com/search?q=关键字&page="&lt;/span>&lt;span class="pun">;&lt;/span></code>
  </li>
  <li class="L1">
    <code>&lt;span class="kwd">for&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="kwd">var&lt;/span>&lt;span class="pln"> i&lt;/span>&lt;span class="pun">=&lt;/span>&lt;span class="lit">1&lt;/span>&lt;span class="pun">;&lt;/span>&lt;span class="pln">i&lt;/span>&lt;span class="pun">&lt;&lt;/span>&lt;span class="lit">101&lt;/span>&lt;span class="pun">;&lt;/span>&lt;span class="pln">i&lt;/span>&lt;span class="pun">++){&lt;/span></code>
  </li>
  <li class="L2">
    <code> &lt;span class="kwd">var&lt;/span>&lt;span class="pln"> request &lt;/span>&lt;span class="pun">=&lt;/span> &lt;span class="kwd">null&lt;/span>&lt;span class="pun">;&lt;/span></code>
  </li>
  <li class="L3">
    <code> &lt;span class="kwd">if&lt;/span> &lt;span class="pun">(&lt;/span>&lt;span class="pln">window&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="typ">ActiveXObject&lt;/span>&lt;span class="pun">)&lt;/span> &lt;span class="pun">{&lt;/span></code>
  </li>
  <li class="L4">
    <code>&lt;span class="pln"> request &lt;/span>&lt;span class="pun">=&lt;/span> &lt;span class="kwd">new&lt;/span> &lt;span class="typ">ActiveXObject&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">"Microsoft.XMLHTTP"&lt;/span>&lt;span class="pun">);&lt;/span></code>
  </li>
  <li class="L5">
    <code> &lt;span class="pun">}&lt;/span>&lt;span class="kwd">else&lt;/span> &lt;span class="pun">{&lt;/span></code>
  </li>
  <li class="L6">
    <code>&lt;span class="pln"> request &lt;/span>&lt;span class="pun">=&lt;/span> &lt;span class="kwd">new&lt;/span> &lt;span class="typ">XMLHttpRequest&lt;/span>&lt;span class="pun">();&lt;/span></code>
  </li>
  <li class="L7">
    <code> &lt;span class="pun">}&lt;/span></code>
  </li>
  <li class="L8">
    <code>&lt;span class="pln"> request&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">open&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">"GET"&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln">url&lt;/span>&lt;span class="pun">+&lt;/span>&lt;span class="pln">i&lt;/span>&lt;span class="pun">,&lt;/span> &lt;span class="kwd">false&lt;/span>&lt;span class="pun">);&lt;/span></code>
  </li>
  <li class="L9">
    <code>&lt;span class="pln"> request&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">setRequestHeader&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">'If-Modified-Since'&lt;/span>&lt;span class="pun">,&lt;/span> &lt;span class="str">'0'&lt;/span>&lt;span class="pun">);&lt;/span></code>
  </li>
  <li class="L0">
    <code>&lt;span class="pln"> request&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">send&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="kwd">null&lt;/span>&lt;span class="pun">);&lt;/span></code>
  </li>
  <li class="L1">
    <code> &lt;span class="kwd">var&lt;/span>&lt;span class="pln"> str &lt;/span>&lt;span class="pun">=&lt;/span>&lt;span class="pln"> request&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">responseText&lt;/span>&lt;span class="pun">;&lt;/span></code>
  </li>
  <li class="L2">
    <code>&lt;span class="pln"> str &lt;/span>&lt;span class="pun">=&lt;/span>&lt;span class="pln"> str&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">replace&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">/\r/&lt;/span>&lt;span class="pln">g&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="str">""&lt;/span>&lt;span class="pun">);&lt;/span></code>
  </li>
  <li class="L3">
    <code>&lt;span class="pln"> str &lt;/span>&lt;span class="pun">=&lt;/span>&lt;span class="pln"> str&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">replace&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">/\n/&lt;/span>&lt;span class="pln">g&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="str">""&lt;/span>&lt;span class="pun">);&lt;/span></code>
  </li>
  <li class="L4">
    <code> &lt;span class="kwd">var&lt;/span>&lt;span class="pln"> urls &lt;/span>&lt;span class="pun">=&lt;/span> &lt;span class="pun">[];&lt;/span></code>
  </li>
  <li class="L5">
    <code>&lt;span class="pln"> str&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">replace&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">/\&lt;div class=\'ip\'&gt;.*?&lt;a href=\".*?\"&gt;(.*?)&lt;\/a&gt;.*?&lt;\/div&gt;/&lt;/span>&lt;span class="pln">ig&lt;/span>&lt;span class="pun">,&lt;/span> &lt;span class="kwd">function&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">a&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln">b&lt;/span>&lt;span class="pun">)&lt;/span> &lt;span class="pun">{&lt;/span></code>
  </li>
  <li class="L6">
    <code>&lt;span class="pln"> urls&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">push&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">b&lt;/span>&lt;span class="pun">);&lt;/span></code>
  </li>
  <li class="L7">
    <code> &lt;span class="pun">});&lt;/span></code>
  </li>
  <li class="L8">
    <code>&lt;span class="pln"> console&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">info&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">urls&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">join&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">'\n'&lt;/span>&lt;span class="pun">));&lt;/span></code>
  </li>
  <li class="L9">
    <code>&lt;span class="pun">}&lt;/span></code>
  </li>
</ol>

新版的shodan：

<ol class="linenums">
  <li class="L0">
    <code>&lt;span class="kwd">var&lt;/span>&lt;span class="pln"> url &lt;/span>&lt;span class="pun">=&lt;/span> &lt;span class="str">"https://www.shodan.io/search?query=port%3A27017&page="&lt;/span>&lt;span class="pun">;&lt;/span></code>
  </li>
  <li class="L1">
    <code>&lt;span class="kwd">for&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="kwd">var&lt;/span>&lt;span class="pln"> i&lt;/span>&lt;span class="pun">=&lt;/span>&lt;span class="lit">1&lt;/span>&lt;span class="pun">;&lt;/span>&lt;span class="pln">i&lt;/span>&lt;span class="pun">&lt;&lt;/span>&lt;span class="lit">101&lt;/span>&lt;span class="pun">;&lt;/span>&lt;span class="pln">i&lt;/span>&lt;span class="pun">++){&lt;/span></code>
  </li>
  <li class="L2">
    <code> &lt;span class="kwd">var&lt;/span>&lt;span class="pln"> request &lt;/span>&lt;span class="pun">=&lt;/span> &lt;span class="kwd">null&lt;/span>&lt;span class="pun">;&lt;/span></code>
  </li>
  <li class="L3">
    <code> &lt;span class="kwd">if&lt;/span> &lt;span class="pun">(&lt;/span>&lt;span class="pln">window&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="typ">ActiveXObject&lt;/span>&lt;span class="pun">)&lt;/span> &lt;span class="pun">{&lt;/span></code>
  </li>
  <li class="L4">
    <code>&lt;span class="pln"> request &lt;/span>&lt;span class="pun">=&lt;/span> &lt;span class="kwd">new&lt;/span> &lt;span class="typ">ActiveXObject&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">"Microsoft.XMLHTTP"&lt;/span>&lt;span class="pun">);&lt;/span></code>
  </li>
  <li class="L5">
    <code> &lt;span class="pun">}&lt;/span>&lt;span class="kwd">else&lt;/span> &lt;span class="pun">{&lt;/span></code>
  </li>
  <li class="L6">
    <code>&lt;span class="pln"> request &lt;/span>&lt;span class="pun">=&lt;/span> &lt;span class="kwd">new&lt;/span> &lt;span class="typ">XMLHttpRequest&lt;/span>&lt;span class="pun">();&lt;/span></code>
  </li>
  <li class="L7">
    <code> &lt;span class="pun">}&lt;/span></code>
  </li>
  <li class="L8">
    <code>&lt;span class="pln"> request&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">open&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">"GET"&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln">url&lt;/span>&lt;span class="pun">+&lt;/span>&lt;span class="pln">i&lt;/span>&lt;span class="pun">,&lt;/span> &lt;span class="kwd">false&lt;/span>&lt;span class="pun">);&lt;/span></code>
  </li>
  <li class="L9">
    <code>&lt;span class="pln"> request&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">setRequestHeader&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">'If-Modified-Since'&lt;/span>&lt;span class="pun">,&lt;/span> &lt;span class="str">'0'&lt;/span>&lt;span class="pun">);&lt;/span></code>
  </li>
  <li class="L0">
    <code>&lt;span class="pln"> request&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">send&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="kwd">null&lt;/span>&lt;span class="pun">);&lt;/span></code>
  </li>
  <li class="L1">
    <code> &lt;span class="kwd">var&lt;/span>&lt;span class="pln"> str &lt;/span>&lt;span class="pun">=&lt;/span>&lt;span class="pln"> request&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">responseText&lt;/span>&lt;span class="pun">;&lt;/span></code>
  </li>
  <li class="L2">
    <code>&lt;span class="pln"> str &lt;/span>&lt;span class="pun">=&lt;/span>&lt;span class="pln"> str&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">replace&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">/\r/&lt;/span>&lt;span class="pln">g&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="str">""&lt;/span>&lt;span class="pun">);&lt;/span></code>
  </li>
  <li class="L3">
    <code>&lt;span class="pln"> str &lt;/span>&lt;span class="pun">=&lt;/span>&lt;span class="pln"> str&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">replace&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">/\n/&lt;/span>&lt;span class="pln">g&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="str">""&lt;/span>&lt;span class="pun">);&lt;/span></code>
  </li>
  <li class="L4">
    <code> &lt;span class="kwd">var&lt;/span>&lt;span class="pln"> urls &lt;/span>&lt;span class="pun">=&lt;/span> &lt;span class="pun">[];&lt;/span></code>
  </li>
  <li class="L5">
    <code>&lt;span class="pln"> str&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">replace&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">/\&lt;div class=\"ip\"&gt;.*?&lt;a href=\".*?\"&gt;(.*?)&lt;\/a&gt;.*?&lt;\/div&gt;/&lt;/span>&lt;span class="pln">ig&lt;/span>&lt;span class="pun">,&lt;/span> &lt;span class="kwd">function&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">a&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln">b&lt;/span>&lt;span class="pun">)&lt;/span> &lt;span class="pun">{&lt;/span></code>
  </li>
  <li class="L6">
    <code>&lt;span class="pln"> urls&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">push&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">b&lt;/span>&lt;span class="pun">);&lt;/span></code>
  </li>
  <li class="L7">
    <code> &lt;span class="pun">});&lt;/span></code>
  </li>
  <li class="L8">
    <code>&lt;span class="pln"> console&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">info&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">urls&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">join&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="str">'\n'&lt;/span>&lt;span class="pun">));&lt;/span></code>
  </li>
  <li class="L9">
    <code>&lt;span class="pun">}&lt;/span></code>
  </li>
</ol>

以上内容整理自：http://zone.wooyun.org/content/16840