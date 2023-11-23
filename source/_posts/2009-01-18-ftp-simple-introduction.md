---
layout: post
title: '[FTP] FTP簡要介紹'
date: 2009-01-18 13:26
comments: true
tags: FTP
---

簡單的FTP(File transfer potocol)介紹…這個應該可以簡單的了解ftp的流程…
<!--more-->
詳細的解說及指令需要去看RFC 959了
基本上，FTP分成二個部分：指令控制port及檔案傳輸port。

指令控制：
一般常用指令如以下

ABOR 中斷之前連接
LIST 列出目錄
PORT 列出可傳輸埠號
QUIT 結束連線
RETR 擷取檔案
STOR 存放檔案
SYST 取得server系統型態
TYPE 改變狀態： A為ASCII模式，I為binary模式

檔案傳輸：
檔案傳輸方式有分成三種，分別為串流、區塊及壓縮模式。

關於FTP server回覆訊息：
訊息分別為以下二種，1種為系統回覆，1種為錯誤訊息
系統回覆：
1xy 準備接受指令
2xy 結束指令
3xy 結束指令但仍需要指令繼續完成
4xy 有問題但可以稍後繼續嘗試
5xy 錯誤但是無法稍後嘗試

錯誤回覆：
x0y 語法錯誤
x1y ftp回覆資訊
x2y 指令結束資訊
x3y 認證資訊
x4y 無列舉資訊
x5y 檔案系統狀態資訊

另外關於使用PORT或PASV指令得到檔案傳輸連結port的資訊該如何得知需要連到那個port..
ex. 10, 38, 2, 103, 8, 102
explane:
10.38.2.103 —>此為IP位置
8*256+102—–>此為port號