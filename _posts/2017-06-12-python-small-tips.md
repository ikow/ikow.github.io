---
id: 4466
title: python small tips
date: 2017-06-12T12:40:35+00:00
author: admin
layout: post
guid: http://blog.ikow.cn/?p=4466
permalink: /python-small-tips/
categories:
  - python
  - 编程
---
# sort dict by value python

To get the values use

<pre class="lang-py prettyprint prettyprinted"><code>&lt;span class="pln">sorted&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">data&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">values&lt;/span>&lt;span class="pun">())&lt;/span></code></pre>

To get the matching keys, use a `key` function

<pre class="lang-py prettyprint prettyprinted"><code>&lt;span class="pln">sorted&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">data&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln"> key&lt;/span>&lt;span class="pun">=&lt;/span>&lt;span class="pln">data&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">get&lt;/span>&lt;span class="pun">)&lt;/span></code></pre>

To get a list of tuples ordered by value

<pre class="lang-py prettyprint prettyprinted"><code>&lt;span class="pln">sorted&lt;/span>&lt;span class="pun">(&lt;/span>&lt;span class="pln">data&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">items&lt;/span>&lt;span class="pun">(),&lt;/span>&lt;span class="pln"> key&lt;/span>&lt;span class="pun">=&lt;/span>&lt;span class="kwd">lambda&lt;/span>&lt;span class="pln"> x&lt;/span>&lt;span class="pun">:&lt;/span>&lt;span class="pln">x&lt;/span>&lt;span class="pun">[&lt;/span>&lt;span class="lit">1&lt;/span>&lt;span class="pun">])&lt;/span></code></pre>