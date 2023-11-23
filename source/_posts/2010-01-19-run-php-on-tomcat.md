---
layout: post
title: '[Quercus]在tomcat上跑php'
date: 2010-01-19 03:28
comments: true
tags: Quercus
---

話說，因為研究了ZK，因為ZK是基於java上的framework，所以如果要使用的話，需跑在tomcat或resin之類的server上。

但是，如果zk想在其他的語言上使用，該怎麼辦？tomcat上只能跑Java嗎？
<!--more-->
其實在早期，一直以為大家都覺得tomcat上只能跑Java，只是後來當一些語言的出現，開始想要在JVM上跑其他的script language而試著不寫Java，於是，出現在很多其他的專案。比較有名氣的像是JRuby、Jython、Groovy、Scala… 只是，最後回頭來想就會有能不能在上頭跑PHP的想法出現？也許是個很瘋狂的想法，但是有了[Quercus](http://quercus.caucho.com/)，就實現了。

「[Quercus](http://quercus.caucho.com/)」是今天的主角。這個其實是另一個web container：Resin底下的一個project。主要是讓Resin上可以除了JSP/Servlet外可以跑PHP。在官方的網頁上建議是跑在Resin上…但是，我讓tomcat上也能玩。因為這個東西，主要是個jar檔。但是目前接下來會介紹的設定方法中大家可以嘗試，只是在實驗的時候，我沒碰到和tomcat有相衝或是問題的地方。請大家注意。



###1. 下載###

先至 Quercus 網站上下載WAR檔。（我下載的是4.0.3版）

###2.複製檔案###

將WAR檔解開，把在WEB-INF/lib資料夾裡頭的「resin」、「inject-16」二個jar檔copy後，貼到自己的web app資料夾。例如自己的資料夾叫「fun」，則放到「fun/WEB-INF/lib」之中。

###3.  設定web.xml檔###

將自己的web app資料夾（目前我們預設叫「fun」）中，在WEB-INF下有一個叫「web.xml」的檔案打開編輯。將以下的資訊放進web.xml之中：

接著進tomcat的管理介面，重新reload一次web app，就會生效了。
{% codeblock lang:xml %}
<servlet>
<servlet-name>Quercus Servlet</servlet-name>
<servlet-class>com.caucho.quercus.servlet.QuercusServlet</servlet-class>
<init-param>
<param-name>license-directory</param-name>
<param-value>WEB-INF/licenses</param-value>
</init-param>
</servlet>

<servlet-mapping>
<servlet-name>Quercus Servlet</servlet-name>
<url-pattern>*.php</url-pattern>
</servlet-mapping>
{% endcodeblock %}

此時只要開始寫php，就可以看的到正確的跑在眼前了XD