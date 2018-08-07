---
id: 111
title: CSCAMP 2014CTF|writeup web-7amama Book
date: 2014-11-24T12:56:27+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=111
permalink: /cscampctf_writeup_web-7amama_book/
categories:
  - CTF
tags:
  - CSCAMPctf
  - CTF
  - writeup
---
<img src="http://kowapp.u.qiniudn.com/cscampctf_writup_web300.jpg" alt="" width="480" height="320" border="0" />

We can see the description first:

Description:

7amamaBook is a social media website where people can sign up and share with each other. It has a bug bounty program and you found a bug and reported it but they refuse to pay you so you want to give them a payback by hacking it.

Then I open the webpage(http://178.63.58.69:8082/bounty.php), I find the web manager post something like this:

### We don&#8217;t pay for CSRF vulnerability.

<div>
  OK, there must have a CSRF vulnerability on this website. Let&#8217;s hack it!
</div>

<div>
  I register a account test233 first. Then user this account to log in this website.
</div>

<div>
  When I check the view-source  of the homepage I find a link: http://178.63.58.69:8082/settings.php
</div>

<div>
  On this page there is nothing to defend the CSRF attack, so we can change anyone&#8217;s password as we can.
</div>

<div>
  I also find another very important link: http://178.63.58.69:8082/profile.php?user=7atata
</div>

<div>
  Sorry, you can&#8217;t view this post.<br /> This post&#8217;s privacy is set to &#8220;Only me&#8221;</p> 
  
  <p>
    This remind us to update the password of the user 7atata.
  </p>
  
  <p>
    And then we get to that page, we find the flag is over there
  </p>
</div>