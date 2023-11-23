---
layout: post
title: '[jQuery]jQuery Gride 使用整理'
date: 2010-08-19 05:02
comments: true
tags: jQuery Gride
---
工作整理，在本文中會使用jQuery Grid的使用以及使用jQuery UI還有幾個需要注意的事項。
<!--more-->
使用版本：

jQuery Grid：3.7.2版 [下載](http://www.trirand.com/blog/) (其實下載後，裡頭會有jQuery 1.4.2。在下載選擇的時候，因為是各模組選擇下載，但是Grid base請「務必」一定要勾選。在本次手作時，我是全勾選下載。)

jQuery：1.4.2版 [下載](http://jquery.com/)

Zend Framework：1.10.7版

使用步驟：

STEP 1. 設定

首先，先用Zend Framework建立一個專案（本範例中名稱為：test）。然後，建立一個Controller為TableController，並且先建立一個TableController使用的view。（位置於application/views/table/index.phtml 此頁為TableController的indexAction預設會讀取的頁面）。設定好後，接下來正式進入使用設定jQuery Grid的步驟：

###1. 加入html至template

在使用jQuery Grid的時候，需要先利用html設定好<table></table>以及<div></div>各一組，主要是讓jQuery Grid將產生出來的表格可以產生在<table></table>中，而分頁的功能部分會放在<div></div>之中。

{% codeblock lang:html %}
<table id="listTable"></table>
<div id="pager"></div>
{% endcodeblock %}

在這段html中，目前先設定table元件的id為listTable，而div元件的id為pager，等一下jQuery Grid設定好後，就會把物件放進去了XD


###2. 加入jQuery library

jQuery Grid需要搭配jQuery來使用。另外，因為jQueryGrid有支援jQuery UI themeRoller，所以我也把這個加進去，讓畫面比較好看點：

{% codeblock lang:php %}
<!– 這行表示加入jQuery Grid的樣式CSS，有一些部分在加入繁中後會有需要調整的地方，在文章後將會再處理 –>
<?php echo $this->headLink()->setStylesheet(‘/js/datagrid/css/ui.jqgrid.css’, ‘screen’, array()); ?>

<!– 這行表示加入jQuery UI themeRoller的sunny的CSS樣式 –>
<?php echo $this->headLink()->setStylesheet(‘/js/theme/css/sunny/jquery-ui-1.8.4.custom.css’, ‘screen’, array()); ?>

<!– 這行表示加入jQuery library –>
<?php echo $this->headScript()->setFile(‘/js/datatables/media/js/jquery.js’, $type=’text/javascript’, $attrs=array()); ?>

<!– 這行表示加入jQuery UI的javascript –>
<?php echo $this->headScript()->setFile(‘/js/theme/js/jquery-ui-1.8.4.custom.min.js’, $type=’text/javascript’, $attrs=array()); ?>

<!– 這行表示加入繁體中文語系顯示 –>
<?php echo $this->headScript()->setFile(‘/js/datagrid/js/i18n/grid.locale-zh_tw.js’, $type=’text/javascript’, $attrs=array()); ?>

<!– 這行表示加入jQuery Grid主要的javascript –>
<?php echo $this->headScript()->setFile(‘/js/datagrid/js/jquery.jqGrid.min.js’, $type=‘text/javascript’, $attrs=array()); ?>

<!– 實作的javascript –>
<?php echo $this->headScript()->setFile(‘/js/dtable.js’, $type=‘text/javascript’, $attrs=array()); ?>
</head>
<body>
<table id="listTable">

</table>
<div id="pager"></div>
</body>
{% endcodeblock %}

注意事項：在jQury Grid的wiki中指出可以使用所附的grid.loader.js做include javascript的動作，就不用在html中加這麼多的文字。在使用前，請先更改該檔案中第四行的「pathtojsfiles」變數，更改成您要加入的javascript的路徑。

###3.  將語系改成繁體中文

因為jQuery Grid所附的i18n的library之中，語系沒有繁體中文，所以需要做一點小處理：

(1). 複製一份在i18n底下的grid.locale-語系代碼.js的檔案在i18n資料夾下，並且將檔案名稱改成：grid.locale-zh_tw.js  (為了方便翻譯，我直接複製了grid.locale-cn.js這個檔案，因為這個是簡中的語系檔，直接翻譯比較快。)

(2). 打開grid.locale-zh_tw.js後，直接會看到有簡中文字的部分，直接修改翻譯成需要的文字後，只要存檔後就可以使用。


###4.  initial jQuery Grid

jQuery Grid資料表切成幾個部分….
![Alt jQuery part](http://www.trirand.com/jqgridwiki/lib/exe/fetch.php?media=wiki:griddesc.png)
>jQuery Grid解說 - 圖片frome http://www.trirand.com/jqgridwiki/

照上圖來看，整個data table會分成四個部分。其中標題的地方是叫「Caption Layer」，每個欄位標題的地方是叫Header Layer。而「body layer」的地方就是顯示資料的地方，而資料的來源是利用ajax向server-side要來的。在最下方Navigation Layer的地方其實就是跟分頁顯示有關係的區塊。而這區塊其實可以加入像是「新增、編輯、顯示資料、重新整理、搜尋」的功能按鍵，會出現在區塊的左邊。

接下來…我會打開dtable.js這個檔案，將Javascript寫在裡頭。(javascript不寫在html裡頭喔！)

{% codeblock lang:javascript %}
$(document).ready(function(){ //當一開始開啟文件ready之後就會直接執行
$("#listTable").jqGrid({ //這行表示在 #listTable這個id的物件中會執行加入jQuery Grid的物件
url: '/Table/table/format/json/', //這行url:的用意是在指定需要自那個url中撈出資料。可支援的資料有xml、json格式。
datatype: 'json', //設定json為接收資料的格式，接收格式資料會在文章後說明
mtype: 'GET', //設定取用資料方式為GET (也可用POST)
colNames:['id', '名稱','連結','備註'], //設定欄位要顯示的標題名稱
colModel :[ //從這邊開始要設定的就是跟欄位本身有關係的設定了.....
{name:'id', index:'id', sortable: false}, //設定第一個欄位為id，並且index設成id為到時候ajax回server side連結時使用的parameter。並且設定為不可做排序。
{name:'name', index:'name', width: 120}, //設定第二個欄位為name，並且設定寬度為120px。寬度沒設定的話，預設為150(值會再經jqGrid再運算過)[colModel屬性說明](http://www.trirand.com/jqgridwiki/doku.php?id=wiki:colmodel_options)
{name:'url', index:'url', align:'right'}, //設定url欄位，這邊是故意設定靠右對齊
{name:'memo', index:'memo', sortable:false}
],

//注意：colNames和colModel的陣列大小要長的一樣，因為這是一對一欄位的設定。如果不一樣的話，在網頁載入時會出現javascript訊息告知你有問題後，jqGrid是沒有辦法呈現的。
pager: '#pager', //設定分頁的位置是出現在id為pager的地方。就是在html中的<div id="pager"></div>;
rowNum: 5,  //設定一開始單頁的筆數，這個欄位值不設定時，預設為一頁20筆。
rowList:[5,10,20], //設定下拉設定一頁顯示筆數，設定採陣列方式，預設空陣列時，畫面上不會出現可設定的下拉選單。
sortname: 'id', //可以設定一開始載入資料時要使用那個欄位來排序，server-side部分要記得接受名稱為「sortname」的變數名稱來處理
sortorder: 'asc', //可以設定一開始載入資料時要使用那種排序方式，server-side部分要記得接受變數名稱為「sortorder」的值來處理。
caption: '===============Grid', //設定table的標題名稱，如果不設定的話，則不會出現table title的部分。
jsonReader : { //此段為設定讀取json的時候該讀那個index
root: "rows", //設定資料欄位陣列是放置在rows這個index之中。
page: "page", //設定目前是第幾頁
total: "total", //設定總共有幾頁
cell: "cell", //設定標示每列資料是放在一個叫cell的index之中。有試過是否可以不設，但是發現資料會顯示不出來。
}
} ).navGrid('#pager',{view:true, del:false, edit: false}) //navGrid在這邊的設定是有關於分頁上一些功能導覽的部分。目前範例裡的設定只有設定有顯示，但是不要出現刪除、編輯這二個功能。所以畫面上會出現搜尋以及顯示整筆資料的功能....
});

{% endcodeblock %}

接下來是json的格式說明，有關xml的部分請見連結：

{% codeblock lang:javascript %}
{
"total": "3″, //這是告訴jqGrid共全部共有幾頁
"page": "1″, //這是告訴jqGrid目前在第幾頁
"records": "13″, //這是告訴jqGrid總共筆數有多少。有的時候自訂navGrid的時候可以顯示出來。

//接下來是資料列的位部分，顯示資料只要照欄位順列一列一列塞進陣列就可以了
"rows" : [
{"cell": ["1", "test1", "http://www.yahoo.com.tw", "testmemo"]},{"cell": ["2", "test2", "http://www.yahoo.com.tw", "test2memo"]},{"cell": ["3", "abc", "http://www.yam.com.tw", "test12345"]},{"cell": ["4", "777", "http://heheh", "adfadfaklj"]},{"cell": ["5", "check name", "adf;lkajdsfklajdf", "memo"]}  ]
}

{% endcodeblock %}

只要在server-side在接收jqGrid的request的時候，產生出這樣格式的json檔案，其實就會將資料顯示上去了。如果json格式出現問題，不見得會出現javascript錯誤，這點要特別注意。為了這個問題，多花了不少debug的時間在上頭…. Orz

接下來要做的事，就是調整一下CSS

###5.  調整CSS

因為自訂繁體中文語系後，會發現一個問題，就是在navigation layer還有header layer的地方，因為高度的問題，字的顯示會很像被切掉一些。為了正確顯示，還是需要解決一下這個問題…..


(1) 打開css/ui.jqgrid.css檔案

(2) header layer的高度，請直接搜尋找到「.ui-jqgrid .ui-jqgrid-htable th div」的CSS，將height的值改大。

(3) navigation layer的地方，每頁筆數的部分可以修改「ui-pg-selbox」的CSS，將height改大一點，比較不會看起來像字被壓在底下。另外就是可以直接鍵入頁數來切換頁面的輸入框，可以修改「ui-pg-input」的CSS將height改大一點，字也不會看起來像被切一半。

這樣就大致大功告成了。如果有需要修改的可以再繼續處理。

###總結：

其實jqGrid基本的功能是顯示資料，另外有其他的功能是需要另外再加掛jqGrid Plugin才能運作。所以其實在使用的時候，官方wiki一開始會先註明是否需要再加其他的plugin的javascript檔，例如：Multi-search (但是這個我一直試不出來，不知道那邊有問題).. 在設定前請記得一定要先看一下jqGrid wiki的說明