---
layout: post
title: '[Bazzar] 環境架設 (使用sftp為protocal)'
date: 2009-05-13 15:50
comments: true
tags: Bazzer
---

安裝軟體：

1. bazaar (在ubuntu裡面套件名稱為bzr)

2. 安裝python套件：python-paramiko

3. Python 2.5 (server上跑2.6的話，在啟動bazaar server後，client端取得log時會出現程式錯誤)
<!--more-->

##開始動工：##

###步驟一：###

先將所需套件裝好，將SSH以及vsftp、bazaar(話說bazaar在Ubuntu的套件裡面是叫bzr)

###步驟二：###

開始要來處理bzr的設定了。

####(1) 先建立整個project要置放的repo在那邊，例如：/var/project 這個資料夾。####

bzr init-repo /var/project

接著，建立一個專案為：prj1在/var/project這個資源庫中。接下來使用以下指令...

{% codeblock lang:text%}
bzr init prj1
bzr add
bzr commit -m "Initial import"
{% endcodeblock %}

這樣第一個專案就產生了。

####(2) 在主機上設定bzr://供使用者可以check out或branch出來####

在主機上鍵入以下指令：

{% codeblock lang:text%}
sudo bzr serve –port=872 –directory=/var/project
{% endcodeblock %}

此行port指定為872(預設bzr://為4155 port)，並且指定資料夾讀取為/var/project這個資料夾為預設。

這樣就已經在server上起好了bazaar的server了。

先在project內建立第一個專案，名稱為prj1

{% codeblock lang:text%}
sudo bzr init prj1
{% endcodeblock %}

接下來就會看到一個專案建立在資源庫中了！

####(3)在client安裝bazaar####

個人在我的clinet (windows xp電腦一台)上安裝的是Qbzr，使用Python 2.5執行。

其實會使用Qbzr原因其實也很簡單，因為不需要裝一堆套件。直接安裝後就可以使用。(但是還是指令型的操作方式，並非是圖形介面)

接下來執行一下Qbzr，來試一下利用bzr提供的protocal看看有沒有成功。(注意，這個時候咱還沒有定ssh..只是確認一下目前架設的環境是成功的。)

在執行Qbzr的時候，下一個指令：

{% codeblock lang:text%}
bzr log bzr://server:port/prj1
{% endcodeblock %}

成功的話，會看到畫面出現有關於prj1這個project的版本訊息。

以上都是利用bzr://為protocal運作。

接下來要使用的話，其實只要直接將bzr://改成用sftp://就可以了。

另外，用sftp://的時候，不需要起bzr server… —>應該不少人看到這句就會想打我….因為其實用了python-paramiko的關係，不用像ssh+bzr這麻煩XD