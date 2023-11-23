---
layout: post
title: 'Mac os 10.8.3如何安裝mysql_python'
date: 2013-05-03 03:28
comments: true
tags: Python
---

其實這篇草稿早在2013/01就寫好了。但一直到今天才有時間整理成中文.....之前在mac上直接安裝python-mysql的時候，一直出現安裝問題。所謂的安裝問題就是發現編譯套件的時候一直裝不起來，在使用的時候即使有辦法import套件但也沒有辦法使用。但因為目前工作環境需要，還是希望能夠好好找到解決方式。解決方法如下：
<!--more-->
###一、首先，使用brew安裝python
(請記住在mac上使用brew install不要使用sudo)
{% codeblock lang:text %}
brew install python  (update to python 2.7)
{% endcodeblock %}

###二、安裝mysql
mysql是必要先被安裝的。可以選擇直接從mysql官網上下載dmg檔或是使用brew來安裝mysql

###三、安裝設定
先從[mysql_python](http://mysql-python.sourceforge.net/)的官網下載source code。接下來，我們需要使用以下的方式來執行安裝：
{% codeblock lang:text %}
python setup.py install
{% endcodeblock %}
接下來會很自動的在編譯後進行安裝。在完成程序後，我們要測試一下是否可以使用：
{% codeblock lang:text %}
> python
> import MySQLdb
{% endcodeblock %}
第一行的作用是進入python指令模式。第二行import的語法才是測試mysql_python是否可以執行。

不過有的時候，會發現某些library無法被載入。所以需要再加幾行到 .bash_profile或是.profile之中...
{% codeblock lang:text %}
MYSQL=/usr/local/mysql/bin
export PATH=$PATH:$MYSQL
export DYLD_LIBRARY_PATH=/usr/local/mysql/lib:$DYLD_LIBRARY_PATH
{% endcodeblock %}

加入後，請記得要source讓設定的路徑生效。這個在mac os 10.8上是可以被運作的。

如果還發生顯示像是「DYLD_LIBRARY_PATH」無法被辨示之類的問題，我們還需要再做一點處理才能讓套件正常運作。這個是個很神奇的問題。當初其實筆者自己使用的時候就是發生找不到「DYLD_LIBRARY_PATH」路徑之類的問題，苦惱了半天還找不到答案。後來，在以下這篇apple官方討論中找到了答案：
[討論內容](https://discussions.apple.com/thread/4143805?start=45&tstart=0)：問題出在dyld: DYLD_ environment variables being ignored because main executable (/usr/bin/sudo) is setuid or setgid. (也是討論內容的標題)這也是造成一直無法正常使用的原因之一。處理方式請照步驟處理，可以暫時解決掉這個問題.....

{% codeblock lang:text %}

sudo sh

< 接下來請鍵入密碼，然後結果會輸出 >

mv /usr/bin/sudo /usr/bin/sudo-real

cat > /usr/bin/sudo

#!/bin/sh

SUDU=/usr/bin/sudo-real

exec $SUDU $* 2>> /dev/null   #一般使用者無法寫出檔案 /var/log/sudu-wrapper-output

^C #不要懷疑，這行是ctrl+c進行跳出動作

sh-3.2# chmod +x /usr/bin/sudo

{% endcodeblock %}
這些處理的作用是多包一層sudo來解決這個問題。這個需要手動一個步驟處理即可解決問題。處理之後，就可以開心的使用mysql_python這個套件了。