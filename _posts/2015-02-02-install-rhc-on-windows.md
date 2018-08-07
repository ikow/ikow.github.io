---
id: 125
title: 【转】Windows本地方式安装OpenShift客户端工具rhc
date: 2015-02-02T01:06:40+00:00
author: admin
layout: post
guid: http://www.ikow.cn/?p=125
permalink: /install-rhc-on-windows/
categories:
  - linux
  - 极客
tags:
  - Openshift
  - redhat
  - rhc
  - windows
---
在Windows环境下搭建OpenShift环境，安装客户端工具rhc，首先需要安装Ruby和Git，参阅<a href="https://developers.openshift.com/en/getting-started-client-tools.html#windows" target="_blank">https://developers.openshift.com/en/getting-started-client-tools.html#windows</a>。

在正确安装Ruby和Git之后，使用RubyGems包管理器（Ruby内置）安装OpenShift的客户端工具rhc。

官方提供的方式为：gem install rhc，但可能是网络原因所致，执行命令后提示错误信息：

> ERROR:  Could not find a valid gem &#8216;rhc&#8217; (>= 0), here is why:
  
> Unable to download data from https://rubygems.org/ &#8211; Errno::ECONNREFUS
  
> ED: No connection could be made because the target machine actively refused it.
  
> &#8211; connect(2) (https://rubygems.org/latest_specs.4.8.gz)

参阅StackOverFlow的一个解答：<a href="http://stackoverflow.com/questions/19745960/unable-to-install-any-gem-by-ruby-in-windows" target="_blank">http://stackoverflow.com/questions/19745960/unable-to-install-any-gem-by-ruby-in-windows</a>

> “This is most likely due to running over a secure (https) connection to rubygems.org. Look at the help for “gem sources –h”, remove the https version and add http://rubygems.org”

问题仍然没有解决。

实际上，gem install支持本地方式安装，即将gem包下载到本地后再执行gem install &#8211;local，参阅<a href="http://stackoverflow.com/questions/220176/how-can-i-install-a-local-gem" target="_blank">http://stackoverflow.com/questions/220176/how-can-i-install-a-local-gem</a>

rhc-1.30.2依赖的gem包列表如下：

  * http://rubygems.org/downloads/archive-tar-minitar-0.5.2.gem
  * http://rubygems.org/downloads/commander-4.2.0.gem
  * http://rubygems.org/downloads/highline-1.6.21.gem
  * http://rubygems.org/downloads/httpclient-2.4.0.gem
  * http://rubygems.org/downloads/net-scp-1.2.1.gem
  * http://rubygems.org/downloads/net-ssh-2.9.1.gem
  * http://rubygems.org/downloads/net-ssh-gateway-1.2.0.gem
  * http://rubygems.org/downloads/net-ssh-multi-1.2.0.gem
  * http://rubygems.org/downloads/open4-1.3.4.gem
  * http://rubygems.org/downloads/rhc-1.30.2.gem

将上述gem文件下载至本地目录下，然后在该目录下执行

gem install rhc &#8211;local .\rhc-1.30.2.gem

执行成功时日志打印如下：

    Successfully installed net-ssh-2.9.1
    Successfully installed net-scp-1.2.1
    Successfully installed net-ssh-gateway-1.2.0
    Successfully installed net-ssh-multi-1.2.0
    Successfully installed archive-tar-minitar-0.5.2
    Successfully installed highline-1.6.21
    Successfully installed commander-4.2.0
    Successfully installed httpclient-2.4.0
    Successfully installed open4-1.3.4
    ===========================================================================
    
    If this is your first time installing the RHC tools, please run 'rhc setup'
    
    ===========================================================================
    Successfully installed rhc-1.30.2
    Parsing documentation for net-ssh-2.9.1
    Installing ri documentation for net-ssh-2.9.1
    Parsing documentation for net-scp-1.2.1
    Installing ri documentation for net-scp-1.2.1
    Parsing documentation for net-ssh-gateway-1.2.0
    Installing ri documentation for net-ssh-gateway-1.2.0
    Parsing documentation for net-ssh-multi-1.2.0
    Installing ri documentation for net-ssh-multi-1.2.0
    Parsing documentation for archive-tar-minitar-0.5.2
    Installing ri documentation for archive-tar-minitar-0.5.2
    Parsing documentation for highline-1.6.21
    Installing ri documentation for highline-1.6.21
    Parsing documentation for commander-4.2.0
    Installing ri documentation for commander-4.2.0
    Parsing documentation for httpclient-2.4.0
    Installing ri documentation for httpclient-2.4.0
    Parsing documentation for open4-1.3.4
    Installing ri documentation for open4-1.3.4
    Parsing documentation for rhc-1.30.2
    Installing ri documentation for rhc-1.30.2
    ===========================================================================
    
    If this is your first time installing the RHC tools, please run 'rhc setup'
    
    ===========================================================================
    Successfully installed rhc-1.30.2
    Parsing documentation for rhc-1.30.2
    11 gems installed