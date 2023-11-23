---
layout: post
title: '[Ant] apache Ant入門'
date: 2011-04-07 06:32
comments: true
tags: Apache Ant
---

這個算是早該寫出來的文章債….. 畢竟要趁還沒忘記第二次的時候快生出來才對。不過這篇會和另一篇phing內容很像(說明幾乎是一樣吧？！)… 只是phing會外加有svn的部分。
<!--more-->

以下教學不含直接使用ant的java class做些處理。純以入門直接使用撰寫build.xml基本操作。入門簡單，但操作在個人。應用就不難了。
有人會問說apache ant到底是在做什麼用的。話說，[Ant](http://ant.apache.org/)其實是一個java的command line工具。這個工具是方便你能完成所指定的工作。[Ant](http://ant.apache.org/)是用java撰寫的一個工具。其實有用過php的phing的話，馬上就會覺得這麼這麼熟悉。不過我是先會Ant再學習[phing](http://www.phing.info/trac/)，所以感覺剛好反過來XDDD 。說真的，這麼好用的工具，怎麼能不會呢？以下我就用ant的方式大概簡要解說一次…..

##一、安裝
需安裝JRE/JDK才能使用。
下載回來後，直接解開後，即可使用。
指令使用為ant，放在ant資料夾下的bin底下。可以直接設成可以直接下ant指令較為方便…..

##二、使用

######1. 執行

(1) 先建立一個檔案叫做build.xml (請記得使用utf-8為編碼)

{% codeblock lang:xml %}
<?xml version="1.0" encoding="UTF-8"?>
<project name="antProject" default="create">
    <property name="targetFolder" value="project" />
    <target name="create">
        <echo>starting prepare ${ant.project.name} folder</echo>
    </target>
    <target name="deploy">
        <echo>continue to deploy to ${targetFolder}</echo>
    </target>
</project>
{% endcodeblock %}

(2) 執行預設target：

{% codeblock lang:text %}	
ant (直接enter)
{% endcodeblock %}

(3). 如果要執行deploy這個target時：

{% codeblock lang:text %}
phing deploy (直接enter)
{% endcodeblock %}


######2. 入門說明

{% codeblock lang:xml %}
<?xml version="1.0" encoding="UTF-8"?>
<project name="antProject" default="create">
    <property name="targetFolder" value="project" />
    <target name="prepare">
        <echo>depend action</echo>
    </target>
    <target name="create" depends="prepare">
        <echo>starting prepare ${ant.project.name} folder</echo>
    </target>
    <target name="deploy" depends="create, prepare">
        <echo>continue to deploy to ${targetFolder}</echo>
    </target>
</project>
{% endcodeblock %}

在project這個tag中包含起來，顯示為這個專案相關的屬性。也代表這個目前要處理的專案。設定一下name辨示專案名稱為何。然後設定好default屬性，這個default代表的是預設Ant執行時，預設會跑的target。

另外，project中會包含幾個target的tag，這些target的tag裡頭是設定任務要達成的一些動作。設定target的name屬性是指任務的名稱，在執行的時候，可以指定執行要跑那個target。在這個範例中，可以看的到有寫了二個target，一個叫做create，另一個叫deploy。在create這個target之中，可以看到，是用echo這個tag，印出訊息。所以在執行create這個 target的時候，會在畫面中顯示出「開始準備antProject專案建立預備folder」。

在${}符號中間帶想顯示的變數，會顯示出所設定的值。像在create這個target中出現的${ant.project.name}代表會顯示在Ant中，project這個tag之中name的屬性。所以在這個範例中，就會顯示出「antProject」這個值。這是第一種變數的寫法。另一種，請看deploy這個target。在這個target之中，有看到${targetFolder}這個變數，這個顯示是設定要顯示在這個範例中 設定的值。而targetFolder其實就是property這個tag中的name。這是另一種可以設定變數的方法。
在create這個target之中，有看到一個屬性叫做「depends」，這個是設定在執行create這個target的時候，需要預先執行那一個target才可以執行。在文件語法上稱做「依賴」，就是指需要先執行完前置的作業後，最後才執行目前要執行的這個target。所以在create這個target之中，會先去執行prepare這個target才會執行create本身的動作。而depends是可以設定一個以上的target。在deploy的depends之中。

######3. 使用外部properties檔案

{% codeblock lang:xml %}
<?xml version="1.0" encoding="UTF-8"?>
<project name="phingProject" default="getEnv">
    <target name="getEnv">
        <echo>load properties files</echo>
        <property file="default.properties" />
        <echo>ftp: ${ftp}</echo>
    </target>
</project>
{% endcodeblock %}

在以上範例內容中，會發現在getEnv這個target之中，有個property的tag，設定為讀入default.properties這個檔案。default.properties內容如下：

{% codeblock lang:text %}
ftp = localhost
db = db.localhost
{% endcodeblock %}

所以當執行getEnv這個target的時候，會讀入default.properties之中設定的屬性，並且在印出ftp位置的時候，會輸出localhost。
更多apache ant支援的tag以及功能，可以參照[說明](http://ant.apache.org/manual/index.html)。