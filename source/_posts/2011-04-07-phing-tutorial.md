---
layout: post
title: '[phing] phing 入門'
date: 2011-04-07 06:39
comments: true
tags: phing
---

java上的apache ant搬到php上就是phing!
功能一樣，但phing有更多支援：git、svn
整體觀念一樣，只是支援tag會有不同，概念是一樣可以套到apache Ant上。
<!--more-->

安裝：
pear channel-discover pear.phing.info
pear install phing/phing

執行方法：

1. 先建立build.xml，以下為範例內容：

{% codeblock lang:xml %}
<?xml version="1.0" encoding="UTF-8"?>
<project name="phingProject" default="create">
    <property name="targetFolder" value="project" />
    <target name="create">
        <echo msg="開始準備${phing.project.name}專案建立預備folder" />
    </target>
    <target name="deploy">
        <echo msg="進行佈署到${targetFolder}資料夾" />
    </target>
</project>
{% endcodeblock %}

2. 執行預設target：

{% codeblock lang:xml %}
phing (直接enter)
{% endcodeblock %}

3. 如果要執行deploy這個target時：

{% codeblock lang:text %}
phing deploy (直接enter)
{% endcodeblock %}

###簡單入門：

build.xml的結構基本長以下這個樣子：
{% codeblock lang:xml %}
<?xml version="1.0" encoding="UTF-8"?>
<project name="phingProject" default="create">
    <property name="targetFolder" value="project" />
    <target name="prepare">
        <echo msg="顯示先動作" />
    </target>
    <target name="create" depends="prepare">
        <echo msg="開始準備${phing.project.name}專案建立預備folder" />
    </target>
    <target name="deploy" depends="create, prepare">
        <echo msg="進行佈署到${targetFolder}資料夾" />
    </target>
</project>
{% endcodeblock %}

在project這個tag中包含起來，顯示為這個專案相關的屬性。也代表這個目前要處理的專案。設定一下name辨示專案名稱為何。然後設定好default屬性，這個default代表的是預設phing執行時，預設會跑的target。

另外，project中會包含幾個target的tag，這些target的tag裡頭是設定任務要達成的一些動作。設定target的name屬性是指任務的名稱，在執行的時候，可以指定執行要跑那個target。在這個範例中，可以看的到有寫了二個target，一個叫做create，另一個叫deploy。在create這個target之中，可以看到，是用echo這個tag，印出訊息。所以在執行create這個 target的時候，會在畫面中顯示出「開始準備phingProject專案建立預備folder」。

在${}符號中間帶想顯示的變數，會顯示出所設定的值。像在create這個target中出現的${phing.project.name}代表會顯示在phing中，project這個tag之中name的屬性。所以在這個範例中，就會顯示出「phingProject」這個值。這是第一種變數的寫法。另一種，請看deploy這個target。在這個target之中，有看到${targetFolder}這個變數，這個顯示是設定要顯示在這個範例中 設定的值。而targetFolder其實就是property這個tag中的name。這是另一種可以設定變數的方法。
在create這個target之中，有看到一個屬性叫做「depends」，這個是設定在執行create這個target的時候，需要預先執行那一個target才可以執行。在文件語法上稱做「依賴」，就是指需要先執行完前置的作業後，最後才執行目前要執行的這個target。所以在create這個target之中，會先去執行prepare這個target才會執行create本身的動作。而depends是可以設定一個以上的target。在deploy的depends之中。


本身phing和ant一樣，可以由外部讀入.properties的檔案，直接將屬性及值寫在.properties之中。

作法：

{% codeblock lang:xml %}
<?xml version="1.0" encoding="UTF-8"?>
<project name="phingProject" default="getEnv">
    <target name="getEnv">
        <echo msg="讀入外部properties檔案" />
        <property file="default.properties" />
        <echo msg="ftp位置：${ftp}" />
    </target>
</project>
{% endcodeblock %}

在以上範例內容中，會發現在getEnv這個target之中，有個property的tag，設定為讀入default.properties這個檔案。default.properties內容如下：

{% codeblock lang:text %}
ftp = localhost
db = db.localhost
{% endcodeblock %}

所以當執行getEnv這個target的時候，會讀入default.properties之中設定的屬性，並且在印出ftp位置的時候，會輸出localhost。


###svn 功能以及作法：

因為SVN tasks 需要依賴PEAR的VersionControl_SVN，所以需要先安裝這個才能順利執行。
(目前安裝時，需安裝VersionControl_SVN-0.3.4，目前為alpha版stable)

先執行以下指令進行安裝：

{% codeblock lang:text %}
pear install VersionControl_SVN-0.3.4
{% endcodeblock %}

接著，只需要在build.xml加上想做的svn動作即可：

{% codeblock lang:xml %}
<?xml version="1.0" encoding="UTF-8"?>
<project name="phingProject" default="svn-checkout">
    <property name="targetFolder" value="project" />
    <target name="svn-checkout">
        <property file="default.properties" />
        <echo msg="讀入設定的properties檔" />
        <echo msg="svn位置：${svn.server}" />
        <echo msg="帳號：${svn.username}" />
        <echo msg="佈署位置：${deploy.path}" />
        <echo msg="開始進行佈署 ======================================" />
        <!-- 將project checkout下來 -->
        <svncheckout svnpath="/usr/bin/svn" username="${svn.username}" password="${svn.password}" nocache="true" repositoryurl="${svn.server}" todir="${deploy.path}" />
    </target>
</project>
{% endcodeblock %}

以上xml我將設定的屬性放在外部的default.properteis檔。這樣較好修改管理。目前這個範例是將project做checkout。所以只要設定好svncheckout，將屬性帶好，再來下phing執行，就會將project checkout到指定的地方。目前這個範例是在本機端做checkout，遠端可用scp task來處理。

實作commit的時候，需要有workingcopy才能執行。沒辦法在一開始執行的時候將程式新增專案到svn上。


本身phing支援的功能不少。相關tag可以直接看[文件](http://phing.info/docs/guide/stable/)

