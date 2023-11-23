---
layout: post
title: '[Python] 如何在Python中顯示中文，並且不會出現編譯錯誤？'
date: 2009-01-20 13:39
comments: true
tags: Python
---

一開始在學習Python的時候，想在程式中顯示中文....
<!--more-->
在寫Python的時候發現文件中出現中文的時候，在編譯時會出現以下錯誤：
{% codeblock lang:python %}
"UnicodeDecodeError: ‘ascii’ codec can’t decode byte 0xb6 in position 0: ordinal not in range(128) "
{% endcodeblock %}
之類的訊息。所以解法如下：

1. 建立一個python檔案名稱為sitecustomize.py。

2. 存到你的python安裝目錄Libsite-packages

3. 檔案內容為：
{% codeblock lang:python %}
import sys sys.setdefaultencoding(‘utf-8′)
# replace with encoding you want to be the default one
{% endcodeblock %}
存好後，重新再編譯就成功了

如果再不行，參照python官方網站 [說明](http://www.python.org/dev/peps/pep-0263/) 有解決方法！