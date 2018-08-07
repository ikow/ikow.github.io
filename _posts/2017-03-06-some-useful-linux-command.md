---
id: 4447
title: some useful Linux command
date: 2017-03-06T04:05:19+00:00
author: admin
layout: post
guid: http://blog.ikow.cn/?p=4447
permalink: /some-useful-linux-command/
categories:
  - linux
tags:
  - linux
---
<pre class="brush: plain; title: ; notranslate" title="">find . -iwholename '*make*' -not -name CMakeLists.txt -delete
</pre>

this command is like the &#8220;cmake clean&#8221; but you should make sure there are no more other files contain make in their name.