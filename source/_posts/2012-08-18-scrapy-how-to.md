---
layout: post
title: '[Python]Scrapy簡要上手'
date: 2012-08-18 04:18
comments: true
tags: Scrapy
---

人生，沒想到離上次寫部落格竟然會隔了快要半年的時間Orz...工作近期忙碌加上健康狀況有點問題～　很多事就變成了持續延後了。不過最近玩了scrapy，可以輕鬆簡單的來搜集資料。(千萬別問我可以做什麼XD)所以在開始之前，除了需要先安裝好python環境，也必需要具備使用easy_install這個python的套件安裝工具。安裝方式就不多做解釋了，接下來就來小分享一下心得：
官方網址為：[scrapy](http://scrapy.org/)

<!--more-->
目前使用的版本為scrapy 0.14.4，python版本為2.7.2。實際上程式在python 2.6運作上也沒有什麼問題就是了。

### 安裝
第一件事，除了要安裝好python的環境之外，請記得先安裝以下相關的套件：

scrapy
lxml
twisted
simplejson
pyopenssl
w3lib
Twisted

安裝後，其實scrapy會變成像指令形式。所以在開始撰寫專案的時候跟執行就會用的到。

### 第一個scraper
##### 建立專案：

{% codeblock lang:text %}
scrapy startproject projectname
{% endcodeblock %}

這樣會建立一個叫做projectname的專案資料夾。裡頭有scrapy所需要的內容。當建立之後，我們來看看裡頭的結構：

{% codeblock lang:text %}

└── scraper
    ├── scraper
    │   ├── __init__.py
    │   ├── __init__.pyc
    │   ├── db.py
    │   ├── db.pyc
    │   ├── items.py　＃資訊欄位設定的檔案
    │   ├── items.pyc
    │   ├── pipelines.py
    │   ├── settings.py　＃設定檔
    │   ├── settings.pyc
    │   └── spiders
    │       ├── __init__.py
    │       ├── __init__.pyc
    │       ├── projectname_spiders.py  #此為主要修改的檔案
    │       ├── projectname_spiders.pyc
    └── scrapy.cfg 設定檔

{% endcodeblock %}
特定幾個檔案會去使用到的部分，大致是這樣的。我們主要撰寫的程式都是在projectname_spider.py這隻程式裡。setting.py設定可以將程式中所需要設定的東西先寫在這邊，再import進程式使用。而scrapy.cfg才是主要是這個專案的設定檔。而items.py內容上會先設定好所需要的欄位資訊進行做定義（例如會想搜隻的資訊如：網址、內容、圖片網址等等。），因為在projectname_spiders.py中我們可能也會使用到它。（就我自己的使用是有先做定義的。）

scrapy的基本架構上大致是這樣。因為本身功能還有不少，單就較為簡單的部分做說明。有興趣的人可以再去官方網站的document上研究。

##### 關於spiders
所有的邏輯，是需要寫在projectname_spider.py中的def parse(self, response)　裡頭。一般來說，要pase html取得所需要的資訊的時候，會需要將html解析取得需要的資訊。
在官方文件中，利用XPath表示的方式如下：

{% codeblock lang:text %}
/html/head/title: 從最外層的html中取得<head></head>之中<title></title>物件

/html/head/title/text(): 取得<title></title>內容

//td: 選取所有的<td></td>

//div[@class="mine"]: 取得全部有class="main" 為屬性的<div>

{% endcodeblock %}

除了text()之外，還有extract()語法。text()是取得tag中的內容。extract()是將tag之中其他的tag以及內容以unicode字串含資料整個回傳。

執行設定要取得那一個網站上的資訊時，首先，需要先設定一下：

{% codeblock lang:python %}
class projectnameSpider(BaseSpider):
    name = "projectname"　＃設定專案名稱
    allowed_domains = ["www.projectname.com.tw"]　＃設定要抓取資料的網址（為一個list）
    start_urls = [
        "http://www.projectname.com.tw/list/1",
        "http://www.projectname.com.tw/list/2",
        "http://www.projectname.com.tw/list/3"
    ]
    # start_urls設定的資料是將要取得的網頁整個先設定好。

{% endcodeblock %}

當start_urls 設定好之後，接下來scrapy就會依著start_urls開始將所有的網址資料進行parse。在parse的時候，就是去執行def parse(self, response) 的內容。

不過這個時候一定會有人問說，如果我爬行的資料網址並不固定的話？不可能在一開始在start_urls中設定好所有我想爬行的資料網址。這時候其實可以import Request (from scrapy.http)，然後

{% codeblock lang:python %}
yield Request(next_url, callback = self.parse)
{% endcodeblock %}

就可以了。


##### 執行方式
我們採command line方式執行：

{% codeblock lang:text %}
scrapy crawl projectname
{% endcodeblock %}

其實projectname 執行的部分就是projectname_spiders.py


##### 結論
其實也只是個心得分享，可以拿來做一些事。不過希望大家使用的時候要往正向的方式使用啊～～～
如果有任何地方有什麼問題，再請大家多多指教。