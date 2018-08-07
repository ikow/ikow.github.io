---
id: 4474
title: Algorithm study notes
date: 2017-09-22T17:23:24+00:00
author: admin
layout: post
guid: http://blog.ikow.cn/?p=4474
permalink: /algorithm-study-notes/
categories:
  - 编程
tags:
  - algorithm
  - study
  - study notes
  - tree
---
Tree:

https://www.cs.cmu.edu/~adamchik/15-121/lectures/Trees/trees.html

&nbsp;

Backtrack:

A general recursive template for backtracking:

<pre class="brush: java; title: ; notranslate" title="">helper (parameters of given data and current recursive level) {
        // Handle base cases, i.e. the last level of recursive call
        if (level == lastLevel) {
            record result;
            return sth;
        }
        // Otherwise permute every possible value for this level.
        for (every possible value for this level) {
            helper(parameters of given data and next recursive level);
        }
        return sth;
    }
</pre>

&nbsp;