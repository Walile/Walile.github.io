---
layout: post
title: '關於php浮點數float以及int的問題'
date: 2013-06-14 04:00
comments: true
tags: PHP
---

這次工作上碰到了一個奇怪的問題。應該說，一般來說大家對浮點數運算上其實沒有太特別注意有什麼不同的狀況。只是覺得最後有小數點就用float，位數不夠就用上double，結果就出事的狀況。而且計算完數字的結果不是多一就是少一。這也太奇怪了，所以這次debug的時候，就連帶把原因也給找了一下。後來發現，其實官網就有特別說明了，但沒有人去注意Warning的那一塊....囧。
<!--more-->
電腦基礎是差分器發展而來的，差分器的運作是採泰勒展開式(線性數學)，採函數逼近法算出結果。(就是微分)。此外，浮點數運算是使用IEEE 754(被廣泛使用的浮點數運算標準)。全稱叫「IEEE二進位浮點數算術標準」根據名稱，我們也會理解它是採二進位方式做運算。php是採用IEEE 754 雙精準確度(64bit)做處理的。

在php官網上有指出，所以計算上在1.11e-16取四捨五入的狀況下就會發生問題。一般表示法常見的是使用10進制的來表示，像0.1, 0.7這種在2進制的狀況下，沒有辦法用很精準的方式來表示。只能用近似的方式表達所以精準度上就會有差異，最終的結果就會不是我們要的：

以下範例：

{% codeblock lang:php %}
echo (0.1+0.7)*10 #答案是8
{% endcodeblock %}

實際運算出來答案會是8, 但以下算完答案會是7，這和我們原先的答案就不同了。

{% codeblock lang:php %}
echo floor((0.1+0.7)*10) #答案會是7
{% endcodeblock %}

正常來說取floor之後怎麼會突然變成7了？正常的答案不是應該是8嗎？
但原因是因為計算出來實際的值其實是：7.9999999999999991118


另外，像是：
{% codeblock lang:php %}
echo 0.1 + 0.2 #真正的答案應該是0.30000000000000004 這個數字是浮點數實際逼近算出來的。php印出來還會是0.3
{% endcodeblock %}

另一個跟int有相關的，範例如下：
{% codeblock lang:php %}
echo (int)((0.1+0.7) * 10) #答案是7，但應該是8才對吧？
{% endcodeblock %}


如果算式是
{% codeblock lang:php %}
echo ((0.1+0.7) * 10)  #答案就會是8了！這個才是我們要的
{% endcodeblock %}

(好大的洞啊.....)

php官方建議如果需要高位數的計算的話，需要使用BC Math或是GMP，網址：
[BC Math](http://www.php.net/manual/en/ref.bc.php)
[GMP](http://www.php.net/manual/en/ref.gmp.php)

參考內容：
[簡單的說明浮點數(php官網提供)](http://floating-point-gui.de/)
[php官網float說明](http://www.php.net/manual/en/language.types.float.php)

(以上的範例都是從php官網相關資料來的)

也許也突顯了一個問題，php的開發人員一開始對於資料類型也許完全沒有特別注意到(畢竟不用一開始就宣告是int, float, double, string .... etc)，也沒有注意到文件底下Warning或Caution的地方，都是先用再說.....但java與其他的語言是否有這樣的狀況，看來要再來試試看。