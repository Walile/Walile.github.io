---
layout: post
title: '[php]zend framework 入門 II'
date: 2010-07-23 04:27
comments: true
tags: Zend Framework1
---
在前一篇寫到建立Action的部分，在這篇將會繼續利用Zend Framework 1.10.6來建立一個簡易的網站，一邊看教學文件為[Tutorial: Getting Started with Zend Framework 1.10 by Rob Allen](http://akrabat.com/zend-framework-tutorial/)，並且一邊註解筆記。
<!--more-->
資料庫處理部分：

我們將會使用Zend_Db_Table來處理有關資料庫資料的add、update、delete功能。不過首先，請先打開application/configs/application.ini，並且找到[production]的段落，進行修改資料庫連結設定：

{% codeblock lang:text %}
resources.db.adapter = PDO_MYSQL
resources.db.params.host = localhost
resources.db.params.username = dbname
resources.db.params.password = dbpassword
resources.db.params.dbname = zf-tutorial
{% endcodeblock %}

這些設定是在設定跟資料庫相關的部分。包含帳號密碼以及資料庫名稱、host、db adapter。然後，需要先在mysql之中，建立一個table

{% codeblock lang:sql %}
CREATE TABLE albums (
id int(11) NOT NULL auto_increment,
artist varchar(100) NOT NULL,
title varchar(100) NOT NULL,
PRIMARY KEY (id)
);
{% endcodeblock %}

並且加入些假資料

Model部分：
------
建立model的部分，需要先繼承Zend_Db_Table和Zend_Db_Table_Row來處理。在Zend Framwork是使用了Table Data Gateway設計模式來處理。而在class的命名上，例如：Application_Model_DbTable_Album 這個class，其實事實在是存放在application/model/DbTable/的Album.php。在教學中，需要先建立一個Model為Application_Model_DbTable_Album的class…接下來加上這幾個method：

{% codeblock lang:php %}
fetchRow("id = ".$id);
    if(!$row){
        throw new Exception("could not find row ".$id);
    }
    return $row->toArray();
}

public function addAlbum($artist, $title){
    $data = array('artist'=>$artist, 'title'=>$title);
    $this->insert($data);
}

public function updateAlbum($id, $artist, $title){
    $data = array('artist'=>$artist, 'title'=>$title);
    $this->update($data, 'id = '.(int)$id);
}

public function deleteAlbum($id){
    $this->delete('id = '.(int)$id);
}
{% endcodeblock %}

Layouts與views：
------
layout可以在指令中指定打開使用layouts。指令為：

{% codeblock lang:text %}
zf enable layout
{% endcodeblock %}

這樣將會自動的建立application/layouts/scripts/layout.phtml (注意，是phtml沒錯)。並且會自動更新application.ini設定預設的layout資料夾的路徑。在layout.phtml設定的東西即可以共同使用。另外，記得css和javascript檔案仍要放在public底下才會有作用。

另外首頁樣板的位置是放在<code>application/views/scripts/index/index.phtml</code>

Action:
------
預設在index中會跑indexAction這個method.add處理的地方在addAction，以此類推。所以處理add、edit、delete都在其所屬的Action中處理以及做驗證。並且記得將最後導回頁面。當這步寫完後，需要去頁面上(該pthml的view)加入

{% codeblock lang:php %}
echo $this->form;
{% endcodeblock %}

這樣才會把在Action中要顯示的form秀在view上頭

基本的CURD大概就是這樣處理。其餘的就待再接觸多點後再來記錄。