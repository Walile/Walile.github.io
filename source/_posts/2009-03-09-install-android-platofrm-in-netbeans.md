---
layout: post
title: '[Android] 在Netbeans上使用Android platform plugin'
date: 2009-03-09 15:44
comments: true
tags: Android
---

在netbeans上安裝Android platform plugin
<!--more-->
使用編輯器：netBeans 6.5

plugin： [android plugin](http://kenai.com/projects/nbandroid/downloads)

##開始##
STEP 1.

先點選在netbeans的「Tools/Plugins」，然後，選擇「Download。接下來會看到有一個「Add Plugins..」的按鍵。先點「Add Plugins..」後，將剛先在plugins 下載下來的檔案全部一個一個加進去後，安裝。

Step 2.

建立一個Android的專案，選擇使用「Android」，然後按下「Next」，接著會發現在「Android platform」的地方會沒有出現Android的platform的字眼。現在請按下「Manage Platforms」，再按下跳出視窗中，有一個「Add Platforms」，選擇「Google Android open handheld platform」，再去選擇從google下載下來的Google Android的SDK資料夾，就大功告成。

##後記：##

剛設定完後，在編譯程式會出現build_impl.xml第440行的錯誤，這時候是因為apkbuilder.jar的位置設定是有問題的。所以接下來先直接去自己下載的android sdk資料夾中，編輯在tools資料夾中的apkbuilder.bat。將frameworkdir和libdir這二個地方填上路徑，就ok了。