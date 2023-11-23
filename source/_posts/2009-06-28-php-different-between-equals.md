---
layout: post
title: '[PHP] 語法中 = 與 == 及 === 有何不同？'
date: 2009-06-28 11:42
comments: true
tags: PHP
---

一般來說應該是需要在學習PHP的時候，就該了解到這三者有什麼樣的不同。後來就特別查了一下。
<!--more-->
一般來說 " = " 在程式語言中，都是指assign的意思。而 " == " (equal) 主要是在比較二個值是否是一樣的。
{% codeblock 比較 == lang:php %}
if("22" == 22){
    echo "true";
}else{
    echo "false";
}
{% endcodeblock %}
這個範例很清楚，在這個範例中，會印出true，因為他們二個的「值」都是22。


此外，" === " (identical) 除了比較二者的「值」之外，還會再針對二者的物件類型還有reference是不是一樣的。如下面的範例：
{% codeblock 比較 == lang:php %}
if("22" === 22){
    echo "true";
}else{
    echo "false";
}
{% endcodeblock %}
結果會印出false。因為"22″和22是不一樣的。"22″是字串類型，而22是Interger，這並不是同一種類別，所以會印出false。但是，如果是 = 的話就不一樣了。因為 = 的意思是assign 。以下程式是常不小心就有可能出現的片段：
{% codeblock 比較 = lang:php %}
if($val = 22){
    echo "true";
}else{
    echo "false";
}
{% endcodeblock %}
這段程式會出現什麼？就是$val的值會變成22。其實很容易在寫程式的時候不小心就會出現這種錯誤，慘的是，這種錯還**常常找不到問題在那邊**。所以其實程式中出現了 if($val = 22)的意思，就會變成是$val的值是22，而不是二者做比較。(註：if("22″ == 22) 的話，會出錯。這樣可以方便在寫程式的時候避免產生錯誤)

參考資料來源：

Difference between '==' (equal) and '===' (identical) comparison operators in PHP (with examples) [techsww_tutorials](http://www.techsww.com/tutorials/web_development/php/tips_and_tricks/difference_between_equal_and_identical_comparison_operators_php.php)