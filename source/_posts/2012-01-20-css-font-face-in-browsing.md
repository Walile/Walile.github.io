---
layout: post
title: '[CSS] 關於網頁中使用CSS中使用嵌入字型'
date: 2012-01-20 03:59
comments: true
tags: CSS3
---

一般來說在設計網站的時候，都是使用預設的系統字型來設計。但如果網站上英文、數字不使用一般系統原有字型。這樣就變成必需要使用另外嵌入的字型了。例如國外的一個服飾網站[A&F](http://www.abercrombie.com)的網站(如下圖)：

![A&F使用](https://s3-ap-southeast-1.amazonaws.com/walilepics/blog/anfscreenshot.jpg)

這些字型，可不是圖片，真的是CSS嵌入字型完成的作品....而且IE6 ~ 9、Firefox、Safari、Chrome、Opera都可以正常顯示。但要滿足這麼多瀏覽器的情況，[A&F](http://www.abercrombie.com)是怎麼樣做到的呢？
<!--more-->
正確的答案就是，使用<code>@font-face</code>！那<code>@font-face</code>要怎麼用呢？請看範例：

#####1. 先在css中定義要使用的字型名稱以及字型檔放置的位置
我們在A&F的CSS原始碼中查看到其中一個字型的<code>@font-face</code>設定如下：
{% codeblock lang:css %}
@font-face{
    font-family:'Trade Gothic Bold'; /** 這個是設定字型的名稱 **/
    src:url('/anf/font/tradegothic-bold-webfont.eot'); /** 設定IE使用的EOT字型檔位置 **/
    src:local('☺'),url('/anf/font/tradegothic-bold-webfont.woff') format('woff'),url('/anf/font/tradegothic-bold-webfont.ttf') format('truetype'),url('/anf/font/tradegothic-bold-webfont.svg#webfontmlgY0et7') format('svg'); /** 設定firefox、chrome...等使用的字型檔放置的地方 **/
    font-weight:normal;font-style:normal; /** 其餘的css設定 **/
}
{% endcodeblock %}

我們可以發現，其實在使用<code>@font-face</code>的時候，「好像」需要很多不同類型的字型格式檔....但是，不要懷疑。**因為真的沒有看錯**！！因為各家瀏覽器在支援的字型格式不同的情況下，所以造成了必須要將希望正常顯示字型的瀏覽器都要設定的情況。
所以，要使用的時候，需要先將<code>@font-face</code>照前面的程式碼先設定好後，再用一般使用CSS設定字型的方式套用，如下方範例：

{% codeblock lang:css %}

.changeType{
    font-size: 30px;
    font-family: "Trade Gothic Bold";
}
{% endcodeblock %}

接下來會看到字型會被套上的樣子...(此字型是套另一個worldwideweb字型)
![在IE8上的字型](https://s3-ap-southeast-1.amazonaws.com/walilepics/blog/ie8.jpg)

#####2. 那些瀏覽器支援什麼樣的字型檔？
基本上，因為常見到的瀏覽器有幾種：IE系列、Firefox、Safari、Chrome。基本上，這幾種瀏覽器都是需要正常顯示出來。初步測試後，支援字型檔的格式如下：

* IE6, IE7, IE8, IE9： 支援EOT字型檔。
* Firefox：支援otf以及ttf、woff
* Safari：支援svg、otf
* Chrome (Google 瀏覽器)：支援svg、otf

#####3. 這幾個英文是什麼意思？
* EOT(Embedded OpenType)： 由微軟制定並於2008年1月提交至W3C的字型規格。因為本篇不是在介紹字型格式，所以詳情可以看[這邊](http://www.w3.org/Submission/2008/SUBM-EOT-20080305/)了解格式的內容。
* OTF(OpenType)：也是由微軟一開始制定，後來adobe也加入共同發展。[字型規格](http://www.microsoft.com/typography/otspec/)
* TTF(TrueType)：早期為了對抗Adobe的Type1 Postcript字體而由Apple、微軟共同開發的一種字體標準。[字型規格](http://developer.apple.com/fonts/TTRefMan/index.html)
* WOFF(Web Open Font Format)：2010年由Mozzila基金會、Opera、微軟一起送出的Web開放字型格式。
* SVG(Scalable Vector Graphics)：這不是一種字型檔，而是基於XML的一種製造二維向量圖形的格式。由[W3C](http://www.w3.org/)制定，是一種開放標準。頁面請見[此處](http://www.w3.org/Graphics/SVG/)

基本上只要特別注意提供支援的格式，其實你也做的到不使用系統預設字型來設計網頁！
