---
layout: post
title: '[PHP] 如何計算日期？ (ex. 前一天或後一天的日期)'
date: 2007-12-07 13:00
comments: true
tags: PHP
---

因為之前一直忘記，所以特別就寫起來....
<!--more-->
{% codeblock 計算2007/12/07的前一天以及後一天的日期 lang:php %}
$date = "2007/12/07";

//先拆出日、月、年的數字
$tmp = explode("/", $date);

//轉換日期為毫秒

$inputdate = mktime(0, 0, 0, $tmp[1], $tmp[2], $tmp[0]);

//換算前一天日期

$lastdate = date("Y/m/d", ($inputdate - 1 * 24 * 60 * 60));

//換算後一天日期

$nextdate = date("Y/m/d", ($inputdate + 1 * 24 * 60 * 60));
{% endcodeblock %}