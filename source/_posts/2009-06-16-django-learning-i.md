---
layout: post
title: '[Django] 入門學習 I'
date: 2009-06-16 16:02
comments: true
tags: Django
---

本篇將開始進行Django入門。在接下來的文章中會建立一個簡單的blog系統。在繼續之前，請檢查是否已經有安裝了Python以及Django，如果還沒有的話，請先安裝後再進行接下來的動作。(本篇為學習文。如果有內文有問題請告訴我，感謝！)
<!--more-->
請注意：基本上，接下來的動作都是以python指令，所以應該是沒有windows或是其他作業系統的問題。唯一要注意的是，如果unix like系統的朋友們可能需要注意一下可能會出現的權限問題。

##1. 建立專案##

這次要建立的專案，專案名稱就叫「blog」，我們可以透過指令建立一個屬於Django的web應用程式。

======================================

我們要開始建立專案。所以指令下了：
{% codeblock lang:python %}
django-admin.py startproject blog
{% endcodeblock %}

指令說明：
{% codeblock lang:python %}
django-admin.py startproject <專案名稱>
{% endcodeblock %}
專案名稱會出現在該專案的資料夾名稱

======================================

建立專案後，會發現專案名稱和資料夾的名稱是一樣的。接下來我們進入blog這個資料夾查詢，會發現有以下幾個檔案：
{% codeblock lang:python %}
\_\_init\_\_.py   # 告知python這個為python的package
manage.py   # 一個命令工列工具，可以從這邊了解更多有關於manage.py的細節。
settings.py   # 設定Django專案，可以從這邊了解如何去做設定。
urls.py   # 有關Django專案url的宣告。可以從這邊了解更多。
{% endcodeblock %}


##2. 設定/啟動開發server##

接下來，先將目前的路徑切換到開發專案資料夾中。照目前自己開發的blog專案放置位置在d:pyblog中。所以利用cmd(Dos模式)切換到d:pyblog 後，鍵入以下指令啟動server功能來查看目前的Django專案。

{% codeblock lang:python %}
python manage.py runserver
{% endcodeblock %}

當啟動成功時，會出現類似如下的訊息：

{% codeblock lang:python %}
Django version 1.0.2 final, using settings ‘blog.settings’
Development server is running at http://127.0.0.1:8000/
Quit the server with CTRL-BREAK.

Django version 1.0.2 final, using settings ‘blog.settings’

Development server is running at http://127.0.0.1:8000/

Quit the server with CTRL-BREAK.

{% endcodeblock %}

這代表目前開發伺服器已啟動，並且預設是用port 8000為連結接口。只需要在瀏覽器上打上http://127.0.0.1:8000/ 並且在畫面上有看到「It’s Worked…」之類的訊息，就代表目前的設定為成功的。

如果想要關閉目前的server設定，只需要在剛剛啟動的Dos模式中，直接按「ctrl + c」就會關閉服務。

但是如果想要指定其他的port進行服務的話，只需要這樣下：

{% codeblock lang:python %}
python manage.py runserver <port號>
{% endcodeblock %}
ex.
{% codeblock lang:python %}
python manage.py runserver 127.0.0.1:8080
{% endcodeblock %}

這樣服務就可以透過本機的8080 port進行服務了

詳細的設定可以見manage.py的runserver篇。


##3. 資料庫設定##

接著要先來設正資料庫。我們要先打開settings.py這個檔案。修改以下個設定：
{% codeblock lang:sql %}
DATABASE_ENGINE = (這邊要填入資料庫的名稱，像是sqlite、mysql、oracle)
DATABASE_NAME = (資料庫的名稱，如果是sqlite為資料庫的路徑。本例中只有設定一個資料庫的名稱，就叫「blog」)
DATABASE_USER = (資料庫使用者名稱。sqlite不使用)
DATABASE_PASSWORD = (這邊要填入資料庫的密碼。sqlite的話不使用)
DATABASE_HOST = (資料庫所在的位置。預設是localhost，sqlite不使用)
DATABASE_PORT = (不填為預設port, sqlite不使用)
{% endcodeblock %}
目前是使用sqlite3為資料庫。sqlite3不需要設定到DATABASE_NAME，有需要資料庫的時候，Django會自動建立一個資料庫出來。

當資料庫的設定結束後，接下來需要一個指令處理：

{% codeblock lang:python %}
python manage.py syncdb
{% endcodeblock %}

指令執行後，Django會開始建立一些東西，還有權限、使用者、群組、網站…等等之類的設定。然後畫面上會跳出來問是否需要安裝Djang的認證系統以及建立一個superuser。在此選擇Yes來安裝這些東西。當完成後會直接回到提示字元上。


本文參考[Django document](http://docs.djangoproject.com/en/dev/intro/tutorial01/#intro-tutorial01)一邊做一邊翻譯一邊寫出來的….
