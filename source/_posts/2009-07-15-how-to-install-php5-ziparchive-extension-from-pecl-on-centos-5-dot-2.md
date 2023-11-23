---
layout: post
title: '[PHP]如何在CentOS 5.2上安裝php5的zipArchive (使用PECL)'
date: 2009-07-15 02:40
comments: true
tags: Centos
---

因為在CentOS 5.2的repo中並沒不能安裝。只好另想辦法…
<!--more-->
第一步：安裝yum的plugin： yum-priorities

先在提示字元下鍵入：
{% codeblock lang:text %}
yum install yum-priorities
{% endcodeblock %}
就會直接去安裝完畢。
接著，直接編輯 /etc/yum/pluginconf.d/priorities.conf 這個檔案。更改或加入以下資訊：

{% codeblock lang:text %}
enable = 1
check_obsoletes=1
priority=N
{% endcodeblock %}

存檔後離開。

第二步：設定EPEL

接著在提示字元下，直接執行：

{% codeblock lang:text %}
rpm -Uvh http://download.fedora.redhat.com/pub/epel/5/i386/epel-release-5-3.noarch.rpm
{% endcodeblock %}

第三步：安裝zipArchive

只需要在提示字元下鍵入：

{% codeblock lang:text %}
yum install php-pecl-zip
{% endcodeblock %}

就大功告成了….

但是…..記得將httpd重起才會生效….