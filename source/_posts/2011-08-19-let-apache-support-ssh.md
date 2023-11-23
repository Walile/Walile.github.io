---
layout: post
title: '[php] php使用ssh連線'
date: 2011-08-19 06:48
comments: true
tags: PHP
---

本篇為使用php來建立ssh連線到其他電腦進行一般事務性的處理工作，不過目前ssh2連線支援需透過PECL安裝來支援。
<!--more-->

做法如下：

####[準備環境]

1. ubuntu 11.04 (這個是個人試驗時使用的環境)

####[架設]

{% codeblock lang:xml %}
apache2 php5-mysql libapache2-mod-php5 mysql-server php5 //本行純為架設LAMP
apt-get install libssh2-1-dev libssh2-php
pecl install ssh2 channel://pecl.php.net/ssh2-version
/etc/init.d/apache2 restart //記得restart才會生效
{% endcodeblock %}

註：ssh2-version意思是指要安裝如ssh2-0.11.2這樣的版本號
因為前置需要先讓系統支援ssh2，所以一些前置的lib需要先行安裝，像libssh2-1-dev，以及讓php支援的libssh2-php
這樣就做完基本就成功了！

