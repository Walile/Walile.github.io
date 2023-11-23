---
layout: post
title: '[Ubuntu] 將vsftpd加上SSL使用'
date: 2009-02-18 15:40
comments: true
tags: Ubuntu
---

vsftpd需要加密，本身有支援SSL，不過這樣做需要有幾個動作：
<!--more-->
STEP 1. 安裝所需軟體

OpenSSL以及vsftpd，在Ubuntu底下請用apt-get install來安裝

STEP 2. 設定認證

建立ssl key
{% codeblock lang:text %}
openssl req -x509 -nodes -days 730 -newkey rsa:1024 -keyout /etc/vsftpd/vsftpd.pem -out /etc/vsftpd/vsftpd.pem
{% endcodeblock %}
這樣會把建立的ssl key存在/etc/vsftpd/vsftpd.pem這個檔案中。

STEP 3. 設定vsftpd
{% codeblock lang:text %}
ssl_enable=YES #代表使用SSL加密
ssl_tlsv1=YES
rsa_cert_file=/etc/vsftpd/vsftpd.pem #設定簽證所在位置
rsa_private_key_file=/etc/vsftpd/vsftpd.pem #設定私錀所在位置
{% endcodeblock %}
STEP 4. 重啟service
重啟vsftpd的服務
/etc/init.d/vsftpd restart
這樣就可以了。