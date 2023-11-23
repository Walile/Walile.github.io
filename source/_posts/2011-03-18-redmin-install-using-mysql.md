---
layout: post
title: '[redmin]架設redmin (使用mysql)'
date: 2011-03-18 06:16
comments: true
tags: Redmine
---

基於工作上研究需要，所以架設redmine，redmine是以Ruby寫的一個專案管理軟體。
<!--more-->

架設時參考了二篇文章：


1. Tsung’s Blog: [將 Redmine 安裝於 Debian、Ubuntu Linux](http://plog.longwin.com.tw/my_note-unix/2011/03/08/redmine-debian-ubuntu-linux-2011)


2. ㄚ凱隨手紀： [Ubuntu 10.04 安裝 Redmine](http://blog.darkhero.net/archives/410)


基本上，目前架設環境是放在Ubuntu 10.10之中，因為套件工具有提供，所以就直接安裝了，步驟如下：

{% codeblock lang:text %}	
sudo apt-get install mysql redmine redmine-mysql apache2 git-core subversion libapache2-mod-fcgid libapache2-mod-passenger
{% endcodeblock %}

因為安裝的時候需要用mysql，所以就使用了redmine-mysql，如果想用sqlite也可以使用redmine-sqlite。也可以查看官方網站有支援的其他資料庫。

接下來，安裝好後，還要再設定一下，因為希望把redmine出現在ex. http://localhost/redmine 這樣

{% codeblock lang:text %}	
sudo ln -s /usr/share/redmine/public/ /var/www/redmine
{% endcodeblock %}

然後，還是需要設定一下，要編輯 /etc/apache2/site-enabled/000-default 這個檔案，加入：

{% codeblock lang:text %}	
RailsEnv production
RailsBaseURI /redmine
{% endcodeblock %}

接下來把apache記得reload，應該就可以看的到redmine的畫面了。
一進去有預設帳密為admin，記得需要先行修改