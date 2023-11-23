---
layout: post
title: MySQL指令備份table
date: 2017-06-19 20:12:34
tags: frontend
---
不小心踩到的雷~ 該記一下：
```
create table `new_table` like `table_name`;
```
會把原來要備份的table的屬性一併寫到建立的新table，像是primiry key, auto_increment之類的。但如果是以下的語法建立備份table的話
```
create table `new_table` select * from `table_name`;
```
雖然會幫忙把要備份的table也一併塞入資料，但是原有的屬性，像是primiry key或是auto_increment也都會不見。連預設的charset也會變成是預設的charset，不會把原來要備份的table的encoding也一併設定好。這點要特別注意。

