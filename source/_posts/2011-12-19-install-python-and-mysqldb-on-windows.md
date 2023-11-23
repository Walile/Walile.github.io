---
layout: post
title: '[python] 在windows環境中設定python以及MySQLdb'
date: 2011-12-19 04:08
comments: true
tags: Python
---

<!--more-->
###一、下載安裝python:
請先到<code>http://python.org/getit/</code>下載windows的installer(目前是使用python 2.7版)

###二、一直下一步安裝:
預設安裝會將python2.7安裝在<code>c:\Python27</code>的資料夾之中。在這個時候，windows上直接開始Dos模式後直接下
{% codeblock lang:python %}
c:\python
{% endcodeblock %}
會出現錯誤，因為這個時候系統並沒有將python認成是內部指令。

###三、設定環境變數:
在<code>系統 / 進階 / 環境變數</code>之中，加入以下變數設定：
{% codeblock lang:text %}
PATH = C:\Python27
{% endcodeblock %}

接下來重開一次<code>Dos模式</code>重新再下一次python指令後，應該會進入python的指令模式。這時候退出請下：
{% codeblock lang:python %}
exit()
{% endcodeblock %}

基本的python環境就完成了。

###四、安裝套件工具easy_install
先到<code>http://pypi.python.org/pypi/setuptools#files</code>下載相對python版本的工具。因為之前筆者安裝的是python 2.7，所以在這邊我安裝了2.7版本的<code>setuptools-0.6c11.win32-py2.7.exe</code>檔。
下載好後，直接點開一直按下一步安裝。除非預設的Python27的資料夾有變動，否則直接下一步安裝就可以完成了。當安裝完畢後，會在<code>C:\Python27\Scripts</code>這個資料夾下發現有<code>easy_install</code>的程式可以執行。

###五、將easy_install加到環境變數之中
因為安裝後還是不能直接在dos環境下直接下指令，所以，就手動把環境變數加進去。
{% codeblock lang:text %}
PATH = C:\Python27\Scripts
{% endcodeblock %}

###六、安裝MySQLdb for Python:
先到這個網址: [http://www.codegood.com/archives/129](http://www.codegood.com/archives/129)下載讓python 2.7使用的檔案。下載好後，一樣直接點二下執行安裝。一樣一直按「下一步」安裝，就會安裝在預設的資料夾<code>C:\Python27</code>之中。 (本文為32bit的安裝方式。64bit會需要更改機碼。)

這樣就完成了。

