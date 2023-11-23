---
layout: post
title: '[freeBSD]在freeBSD上安裝webmin以及bazzar'
date: 2009-07-13 02:09
comments: true
tags: FreeBSD
---

在FreeBSD上安裝webin。首先先來安裝bazaar。但是因為bazaar是基於python上的東西，所以必需要先安裝python才行。

接下來的安裝都會透過ports系統來處理，先來說明一下如何安裝python。(目前使用python2.5版，FreeBSD為7.2版)
<!--more-->
在提示字元底下，直接下以下指令：

{% codeblock lang:text %}
cd /usr/ports/lang/python25/ && make install clean
{% endcodeblock %}

python25是指版本，其他版本可以在這邊更改。接著，只要按下enter後，就會直接進行安裝動作。

注意：請記得先取得管理者權限。

然後，現在再來安裝bazaar

{% codeblock lang:text %}
cd /usr/ports/devel/bazaar/ && make install
{% endcodeblock%}

安裝完畢，現在來安裝的是openldap

{% codeblock lang:text %}
cd /usr/ports/net/openldap24-server/ && make install clean
{% endcodeblock %}

安裝後，要設定幾個東西：

1. 先修改/usr/local/etc/openldap/sldap.conf 加入以下的schema

{% codeblock lang:text %}
include         /usr/local/etc/openldap/schema/core.schema
include         /usr/local/etc/openldap/schema/cosin.schema
include         /usr/local/etc/openldap/schema/misc.schema
include         /usr/local/etc/openldap/schema/inetorgperson.schema
include         /usr/local/etc/openldap/schema/openldap.schema
include         /usr/local/etc/openldap/schema/nis.schema
{% endcodeblock %}

另外，再寫入存取限制

{% codeblock lang:text %}
access to attr=這是密碼
by self write
by anonymous auth
by dn.base="cn=Manager,dc=books,dc=com,dc=tw" write
by * none
access to *
by self write
by users read
by anonymous read
by dn.base="cn=Manager,dc=books,dc=com,dc=tw" write
by * none
{% endcodeblock %}

然後記得去更改suffix的dc以及rootpw的密碼，就完成設定了。(密碼可以用明碼，或是用slappasswd產生一個加密後的密碼再填上去)

最後，啟動一下openLDAP

{% codeblock lang:text %}
echo ‘slapd_enable="YES"‘ >> /etc/rc.conf ; /usr/local/etc/rc.d/slapd start
{% endcodeblock %}

大致上告一段落了。接著就是要處理webmin的安裝了。webmin，本身也可以直接透過ports安裝所以在提示字元下直接可以下下面的指令：

{% codeblock lang:text %}
cd /usr/ports/sysutils/webmin && make install clean
{% endcodeblock %}

接著系統就會開始安裝了。

然後，要來設定啟動webmin來管理系統了

{% codeblock lang:text %}
/usr/local/lib/sebmin/setup.sh     #這個是先做設定的動作
{% endcodeblock %}

待安裝完並且設定後鍵入以下指定啟動

{% codeblock lang:text %}
/usr/local/etc/webmin/start
{% endcodeblock %}

停止系統使用以下指令

{% codeblock lang:text %}
/usr/local/etc/webmin/stop
{% endcodeblock %}

接著，已經可以看到webmin了。

http://ip:設定的port number(預設是10000)

ex. http://localhost:100000

就可以看的到webmin的畫面了。