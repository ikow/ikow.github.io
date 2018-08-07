---
id: 4439
title: ROS study note
date: 2017-01-27T11:32:57+00:00
author: admin
layout: post
guid: http://blog.ikow.cn/?p=4439
permalink: /ros-study-note/
categories:
  - robots
tags:
  - robots
  - Robots Operation System
  - ROS
  - VIM
---
You can choose any editor you like to implement you ROS project. There are some official IDE configuration for ROS : http://wiki.ros.org/IDEs

I prefer using VIM. There is an VIM plugin named rosvim we can use. To install it:

(I use spf13-vim so it uses vundle to manage VIM plugin)

<pre class="brush: bash; title: ; notranslate" title="">$ echo Bundle \'taketwo/vim-ros\' &gt;&gt; ~/.vimrc.bundles.local
$ vim +BundleInstall! +BundleClean +q
</pre>

When I run roscore (ROS master) on the sensor, then I try to run &#8220;rosnode echo rosout&#8221; to print the information of the rosout. It show &#8220;Couldn&#8217;t find an AF\_INET address for &#8220;. So we should set the ROS\_IP on our host like &#8220;export ROS_IP=169.254.10.169&#8221;.

&nbsp;