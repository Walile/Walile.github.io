---
layout: post
title: AWS CloudSearch研究筆記
date: 2016-09-27 00:10:24
tags: Python
---
因為之前公司需求，研究了AWS的cloudsaerch以及Elastic search二套。這篇就之前研究的cloudsearch的部分做一個簡單的筆記。Amazon AWS的cloudsearch是一個讓人能夠簡單建立一個搜尋的服務。在中文分詞的部分是已建立了...但倒是有一件很重要的事情就是，中文搜尋的部分，基本上一定要建入「**二個中文字**」才能進行搜尋。因為一個中文字，在搜尋中是不被認成是一個詞，這點是在使用上最重要要注意的部分，接下來就細節建議進一步說明。

<!--more-->
在一開始要建立一個cloudsearch的時候，我們必需要先建立一個domain。在cloudsearch中，一個domain，我們當成是一個大的table。所有的資料都是在這個table中搜尋。接下來每一個在cloudsearch中的index則為一個欄位。這點會方便我們在一開始進入使用cloudsearch的時候不清楚該如何操作時，利用了一點RMDB的觀點來理解(但這並非完全對或錯)。這個方式和之前在看Elastic search的時候有一個對照在理解操作方式：


| RMDB | DB    | Table | rowcolumn |        |
| ---- |-------|-------|-----------|--------|
|  ES  | Index | Type  | document  | fields |

** ES指的是Eastic Search

接下來在觀念較為清楚的狀態下，我們要再注意一點，就是在search的條件。我們一般在做搜尋是資料精確度由最精準到愈不精準排序下來的。在AWS中也是有地方可以調整自己的ranking pattern的，這個部分要另外再做研究了。

在Cloudsearch中可以使用輸入search的方法照[官方文件](https://docs.aws.amazon.com/cloudsearch/latest/developerguide/searching.html)中有幾種：
1. simple - 籍由較簡單的url parameter的方式把想搜尋的資料丟進去搜尋。
2. structured - 除了simple的方式外另外加上可以特別指定搜尋的欄位以及一些綜合條件。
3. lucene - 不用多說，就是提供和[Apache Lucene](https://lucene.apache.org/)的語法條件方式讓使用者使用
4. dismax - 除了使用Apache Lucene的解析方式以及dismax的語法

這幾個方式並沒有特別的好或是不好，以自己在開發時所需要的方式使用。最後二個因為是提供lucene的語法，在AWS的文件上並沒有特別說明，這個部分就要去Apache Lucene去查語法來使用。

以最基本來說，能夠將資料完整倒進去，就可以完成一個很簡單的搜尋功能，其餘就要看怎麼發揮了....






