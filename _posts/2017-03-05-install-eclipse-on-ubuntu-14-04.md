---
id: 4445
title: install eclipse on ubuntu 14.04
date: 2017-03-05T06:47:00+00:00
author: admin
layout: post
guid: http://blog.ikow.cn/?p=4445
permalink: /install-eclipse-on-ubuntu-14-04/
categories:
  - 编程
tags:
  - eclipse
  - jdk
  - jre
  - ubuntu
  - ubuntu14.04
---
before install, you should update your JRE and JDK to 8:

## Final Update

**JDK**

<pre class="lang-java prettyprint prettyprinted"><code>&lt;span class="pln">sudo apt&lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">get install openjdk&lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="lit">8&lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">jdk&lt;/span></code></pre>

**JRE**

<pre class="lang-java prettyprint prettyprinted"><code>&lt;span class="pln">sudo apt&lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">get install openjdk&lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="lit">8&lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">jre&lt;/span></code></pre>

### Old Update

I found two repository but I do not recommend

  * _OpenJDK builds (all archs)_ <pre class="lang-java prettyprint prettyprinted"><code>&lt;span class="pln">ppa&lt;/span>&lt;span class="pun">:&lt;/span>&lt;span class="pln">openjdk&lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">r&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">ppa&lt;/span></code></pre>

  * _OpenJDK 8 backport for trusty_ <pre class="lang-java prettyprint prettyprinted"><code>&lt;span class="pln">ppa&lt;/span>&lt;span class="pun">:&lt;/span>&lt;span class="pln">jochenkemnade&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">openjdk&lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="lit">8&lt;/span></code></pre>

* * *

### Original Message

If you really want to use OpenJDK, you have to [compile](https://github.com/hgomez/obuildfactory/wiki/How-to-build-and-package-OpenJDK-8-on-Linux) from source. There is not still any PPA for OpenJDK.

It has been requested at <https://bugs.launchpad.net/ubuntu/+bug/1297065>

I recommend you to use [Webup8 Oracle Java8 Installer](http://www.webupd8.org/2012/09/install-oracle-java-8-in-ubuntu-via-ppa.html)

<pre class="lang-java prettyprint prettyprinted"><code>&lt;span class="pln">sudo add&lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">apt&lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">repository ppa&lt;/span>&lt;span class="pun">:&lt;/span>&lt;span class="pln">webupd8team&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">java &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">y
sudo apt&lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">get update
sudo apt&lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">get install oracle&lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">java8&lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">installer&lt;/span></code></pre>

To automatically set up the Java 8 environment variables

<pre class="lang-java prettyprint prettyprinted"><code>&lt;span class="pln">sudo apt&lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">get install oracle&lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">java8&lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">set&lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="kwd">default&lt;/span></code></pre>

Check it

<pre class="lang-java prettyprint prettyprinted"><code>&lt;span class="pln">java &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">version&lt;/span></code></pre>

So you have to wait to use OpenJDK8

&nbsp;

Then download eclipse from https://www.eclipse.org/downloads/

unpack and install it