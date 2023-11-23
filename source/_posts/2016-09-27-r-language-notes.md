---
layout: post
title: R語言設定以及基本使用概念
date: 2016-09-27 00:11:24
tags: R-language
---
因為修課的關係所以需要記錄一下：
<!--more-->
## 安裝
很簡單，只需要去官網下載即可。下載後有預設編輯器可以使用。如果要使用command line也行，只需要執行
```
R
```
沒看錯，打進去就會啟動R language的command line mode。
想要hello world只要
```
print("hello world")
```
這樣執行就可以了。

## 語言轉換重點
1. 語法有python的味道，陣列以vector型式處理。vector index從1開始算(很多語言都是用0開始)
2. 陣列range可以這樣顯示[3:5] 印出3, 4, 5
3. 一個陣列可以直接印出一個範圍: sentence([3:5]) ->印出sence中index3到5的內容
4. array value assign的時候可以一次assign到幾個index中: setence[3:5] <- c('1', '2', '3')
5. 在語言中「<-」是assign的意思(也可以用=來代表assign)
6. 對了，跟Python一樣結尾可是沒有分號的喔！

## package的安裝與使用

套件工具：CRAN。要安裝套件的時候請將以下指令請在R的command mode裡面執行
```
install.package("套件名稱")
```
第一次執行會需要選擇要下載的預設伺服器。接下來就會安裝完成。完成後，就可以用以下的方法開始使用：
```
require("套件名稱")
```
或是
```
Library("套件名稱")
```
如果不確定是否有安裝，也可以用這個指令來確認安裝細節
```
installed.packages("套件名稱")
```

## 如何執行外部寫好的r script?
如果是在官方提供的R編輯執行器，請在command mode底下執行：
```
source("完整檔案路徑")
```
BUT, 人生就是這個BUT，當如果是利用terminal console之類的，可以切換到要執行的script的路徑底下，直接執行：
```
source("檔案名稱")
```
這樣就可以執行寫好的script了。

