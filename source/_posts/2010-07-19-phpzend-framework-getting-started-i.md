---
layout: post
title: '[php]zend framework 入門 I'
date: 2010-07-19 03:56
comments: true
tags: Zend Framework1
---
Orz…又是看到一直忘記要記下來的筆記…..

因為工作需要，開始碰zend……入門就要寫簡單上手，然後，接下來開始剖析一下(謎：好像還沒真的寫到進階的文章ㄟOrz…)
<!--more-->
接下來的內容將會利用Zend Framework 1.10.6來建立一個簡易的網站，一邊看教學文件為Tutorial: Getting Started with Zend Framework 1.10 by Rob Allen，並且一邊註解筆記。

接下來的內容將會利用Zend Framework 1.10.6來建立一個簡易的網站。

基本需求：

>PHP 5.2.4以上


>apache 需要支援mod_rewrite

---------
另外，需要將http.conf中的設定AllowOverride None改成AllowOverride All。而且還需要先設定讓apache可以使用.htaccess檔案進行設定。然後，先來下載zend framework。當下載後，會看到是一個壓縮的檔案(zip或tar.gz)。

接下來，現在就先來設定zend tool…..

其實照著教學說明中，是將剛下載回來的zend framework解壓縮後，放置在在C:Program FilesZendFrameworkCli 的資料夾之中。解開後放到ZendFrameworkCli的資料夾中，會有一個叫bin的資料夾。將這個資料夾的路徑，加到環境變數的PATH之中。正確加好後，打開command line直接打上「zf show version」後按下enter，會看到在畫面中出現所下載的zend framework的版本。

在這分教學中，主要是帶領初心者製作一個可以編輯管理的CD列表。畫面上需要的，就是主畫面、編輯、刪除、新增三個部分。主畫面用來列出所有CD列表並且提供可以進行新增、修改、刪除的連結功能。所以現在我們會開始建立一個專案。利用以下的指令：

{% codeblock lang:php %}
zf create project 專案名稱(資料夾名稱)
{% endcodeblock %}

所以我們的專案叫zf-tutorial的時候，指令就是如下下法：
{% codeblock lang:text %}
zf create project zf-tutorial
{% endcodeblock %}
當按下enter後，會看到zend framework把產生了些資料夾結構和一些檔案。其中在public資料夾中，也會含有所產生的.htaccess檔案。
所產生的結構如下： (有MVC結構)

![Alt zendframework](http://farm8.staticflickr.com/7020/6514182313_44e0979e80_z.jpg)

其中在資料夾中會包含的檔案如下：
![Alt zendframework_file](http://farm8.staticflickr.com/7006/6514192121_d58c52c749_z.jpg)

在以上結構中，可以發現，如果將zend framework解開放置後，其實會讓用戶連進來的時候，網址會如同：http://localhost/zf-tutorial/public/ 才能看的到這個project底下的網頁。所以，其實可以透過將apache的httpd.conf修改，讓根目錄直接指向public資料夾，就不需要還要在網址中特意指定到public才看的到網頁。如http://127.0.0.1/ 就可以看的到了(其實就是設定一個virtual host)。修改方式如下：

{% codeblock lang:php %}
DocumentRoot d:/WEB/wwwroot/zf-tutorial/public/
ServerName zf-tutorial.localhost

AllowOverride All
{% endcodeblock %}

改完後請記得重新啟動apache即生效。
待重新啟動後，直接打上http://127.0.0.1後，就會看到在d:/WEB/wwwroot/zf-tutorial/public/ 了。
在目前最後，需要再將剛下載解開後的zend framework之中的library/Zend的資料夾複製一份後，放到zf-tutorial/library/之中。這樣就安裝完成了。
可以直接在http://127.0.0.1/ 之中看到zend framework的畫面….
在zend framework之中，首先要先了解到一個東西：Bootstrap
在zend framework中使用了front controller這個設計模式，在所有的request進來的時候，可以將其通過index.php設定基本參數設定，確保在每個環境設定都是一致的。而Bootstrap我們稱為引導文件(大陸譯詞)。其中，我們可以經由public/.htaccess設定一進入資料夾即執行public/index.php。
在public/index.php之中，可以看的到在source code部分，有像是APPLICATION_PATH路徑以及APPLICATION_ENV用來定義application資料夾以及環境設定。


Zend_Application 通常是用來啟動和設定application/configs/application.ini。這個檔案是create project的時候自動產生的。另外，會看到一個叫application/Bootstrape.php，這裡頭的Bootstrape class是繼承library/Zend/Application/Bootstrap/Bootstrape.php的Zend_Application_Bootstrape_Bootstrape。而application.ini其實會存在application/configs的資料夾中，透過Zend_Config_Ini這個compoment存取。Zend_Config_Ini可以透過一個繼承的關係，去針對段落讀取處理。舉列：
{% codeblock lang:php %}
[staging: production]
{% endcodeblock %}
這會出現在application.ini之中，在之下到下一個有這樣的段落前，會被Zend_Config_Ini做處理。而其中在「:」之前為段落為此段落的名稱。其後代表有繼承的段落名稱。在這行代表的意思是staging段落會繼承production的段落設定。目前如果在開發過程中需要做debug，請直接在public/.htaccess的第一行加入：
{% codeblock lang:php %}
SetEnv APPLICATION_ENV development
{% endcodeblock %}

接下來要開始先來設定application.ini檔了。

設定application/configs/application.ini檔案其中的時區，請設定以下項目
{% codeblock lang:php %}
phpSettings.data.timezone = "Asia/Taipei"
{% endcodeblock %}
其中"Asia/Taipei"請設定所在城市設定。基本上設定應該是和php.ini的timezone設定一樣。

##URL的結構：

這邊需要先解說一下URL結構的部分。因為zend framework是web mvc(何謂web mvc，請見此連結)結構。在url rewrite的時候有一些和一般不同的判讀：

ex. http://localhost/zf-tutorial/public/news/view

在這樣的URL路徑中，public底下主要是我們會連結到的部分。而news其實是Controller，而View為為Controller之中的Action(在controller之中的method)。預設為index的部分則不會直接顯示在url之中。

##正式實作：

在根據教學pdf中，會實作四個畫面。分別是home、add、update、delete功能。並且利用IndexController外加四個Action(將寫在IndexController之中)實作簡單的管理CD功能。

先來解說Controller的部分。一般Controller都是放在application/controllers的資料夾。命名方式為：「{名稱}Controller.php」其中Controller一定要寫，並且C要大寫。前頭加上命名名稱。而Action的部分，其實是小寫字母外加上Action為一個method。例如，已新增的Controller之中，會看到像是「addAction」為一個method的名稱。接下來，先來新增add、update、delete三個Action在indexController之中：

指令如下：

{% codeblock lang:php %}
zf create action add Index

zf create action update Index

zf create action delete Index
{% endcodeblock %}

我們拿「zf create action add Index」來解釋其中意思代表 要zf建立一個action為Add的Action，並且是加在Index的Controller之中。(記得大小寫需一致)接著，在URL上的顯示，就會變成為：http://localhost/zf-tutorial/public/Index/add 所以，在執行的時候，就會去執行IndexController的addAction這個method了。