---
layout: post
title: '[Ubuntu] 利用vsftpd結合mysql管理ftp使用者(vsftpd虛擬使用者)'
date: 2009-02-15 15:34
comments: true
tags: Ubuntu
---

看了網路上一票文章，做出來的都有問題，後來發現原因出在改錯PAM檔結果不是因為出錯後來連root使用者都無法登入，要不然就是無法使用自己在mysql中建立的使用者進行登入但是找不到問題點。
接下來是這次實作後的結果：(我預設是mysql可以管理使用者，但是系統原來的使用者也可以登入為前提)
<!--more-->

##說明：##
提供一個讓每次主機管理人員一個一個做開立系統帳號及設定權限動作將會讓管理人員帶來管理上煩惱的解決方案。
##環境：##
系統: **Ubuntu 8.10**
vsftpd和mysql

模組請安裝pam_mysql.so

##設定前說明：##
其實這要綁定vsftpd是利用unix帳號做登入帳號管理，所以想利用mysql做管理的話(一般叫做建立vsftpd虛擬使用者)，必需要透過linux中做認證管理的PAM(Pluggable Authentication Modules)做處理，這樣才能將vsftp+mysql+unix帳號綁定管理。

## 開始進行: ##
STEP1. 先下載vsftpd以及mysql先安裝好….(unbuntu 不用說吧？apt-get install 套件名稱)

STEP2. 設定/etc/vsftpd.conf 檔，將設定檔加上以下二個個設定：

guest_enable=YES
guest_name=使用者名稱，本例使用名稱為virtual (這個使用都必需是在unix account中建立的，之後要提供給使用者使用的)

STEP3. 設定mysql：
mysql中，先在mysql資料庫的user這個table中，建立一個專門讓pam認證使用登入的使用者，在本例中使用的名稱叫virtual，並且只能做select的動作，並且用localhost登入。
接下來，建立一個db叫做vsftpd，裡面建立了一個table叫做users，這個是要用來管理虛擬使用者的table。其中的table我建立了username和passwd二個欄位(注意！如果有使用加密，像mysql的password加密欄位，其password欄位請開超過60，否則會認證失敗)。記得要先在這個table中先建立一個使用者，等一下用來測試用。(ex. user，密碼暫設為pass)

STEP4. 開始來設定pam了….
/etc/pam.d/common-auth
這個檔案要加入以下這個
{% codeblock lang:text %}
auth sufficient pam_mysql.so user=virtual passwd=pass host=localhost db=vsftpd table=users usercolumn=username passwdcolumn=passwd crypt=2
{% endcodeblock %}

/etc/pam.d/common-account 這個檔案要加入的
{% codeblock lang:text %}
account sufficient pam_mysql.so user=virtual passwd=pass host=localhost db=vsftpd table=users usercolumn=username passwdcolumn=passwd crypt=2
{% endcodeblock %}

其中，在pam_mysql.so後面出現的user=virtual和passwd=pass是指在mysql中mysql資料庫user這個table中，剛剛有設定可以登入mysql的使用者帳號及密碼。另外，crypt=2代表密碼是用mysql password hash做為加密。(1為unix DES加密、2為mysql password hash加密、3為MD5加密、4為SHA)若要使用SHA加密，其pam_mysql.so的版本需要0.7以上版本才有支援(但是目前都為RC版本)
接下來，重啟服務：
{% codeblock lang:text %}
/etc/init.d/vsftpd restart
{% endcodeblock %}
這樣就完成了！

接下來，進一步想要讓虛擬帳號移到自己的資料夾中，只需要做接下來的動作：
在/etc/中建立一個資料夾為vsftpd_user_config，然後在/etc/vsftpd.conf中，加入二行：
{% codeblock lang:text %}
user_config_dir=/etc/vsftpd_user_config
user_sub_token=$USER
{% endcodeblock %}
然後，在vsftpd_user_config這個資料夾中，建立以虛擬使用者為名稱的檔案。ex. user。然後檔案內只需要加一行來指定登入後要移至的位置：
{% codeblock lang:text %}
local_root=/home/user
{% endcodeblock %}
再重啟服務就完成了！