---
layout: post
title: 'Perl 入門轉換'
date: 2014-02-09 10:51
comments: true
tags: Perl
---
世事多變化...以前因為寫PHP，對於PHP的符號愈來愈多的狀況下，我的內心曾經一度有一個願望：不要再碰到符號很多的語言了....沒想到.....新工作開始被抓去寫perl... (不知道有多少人會想問為啥我還是寫了，實際上碰了perl之後，發現有不少有趣的部分)對於一開始寫perl老是卡卡的狀況，所以還是把常碰到的問題先記下來，日後避免常被一堆符號搞混....
<!--more-->
注意：目前玩的部分是perl 5.14

###宣告變數
可在檔頭一開始宣告: 「use strict」強制使用嚴謹模式。嚴謹模式下，使用變數都需要事先宣告才可以使用。宣告方式如下

{% codeblock lang:perl title:宣告perl變數 %}
my $foo = "hello world!";
{% endcodeblock %}
不事先宣告就直接將變數拿來使用就會出現直譯錯誤。(基於在php上看過的問題，我還是認為加上strict mode是比較好規範自己撰寫方式的一個辦法。)

###變數分類
1. 分成scalar、hash、array
2. 有pointer
3. 有reference

非常感謝同事指導，利用以下的範例讓我搞清楚一堆符號是什麼作用Orz...

####陣列
{% codeblock lang:perl title:perl array example %}
my @array = (1, 2, 3, 4, 5); #宣告一個陣列
# 上面這行可以寫成 my @array = qw/1 2 3 4 5/;
print $array[2]; #印出index2的值
print @array;   # 會印出array size
{% endcodeblock %}

####陣列的ref
{% codeblock lang:perl title:perl array reference example %}
my $array_ref = \@array; #取得array的reference
$array_ref = [1, 2, 3, 4, 5]; #定義array reference裡的值
print $array_ref->[2];   #印出array reference中index為2的值
{% endcodeblock %}

####hash
{% codeblock lang:perl title:perl hash example %}
my %hash = (1 => 'a', 2 => 'b', 3 => 'c', 4 => 'd', 5 => 'e'); #宣告一個hash
print $hash{2}; #印出index2 -> b
{% endcodeblock %}

####hash的ref
{% codeblock lang:perl title: perl hash reference example %}
$hash_ref = \%hash     
$hash_ref = {1 => 'a', 2 => 'b', 3 => 'c', 4 => 'd', 5 => 'e'}
print $hash_ref->{3}
{% endcodeblock %}
###沒有switch
這件事真的讓我一度覺得很囧，但說實在話，python也是啊，所以還是不要讓這個原因變成我學習的跘腳石。這不是重點啊~
(而且其實perl有模組可以達到這件事)

###迴圈
#####for寫法

{% codeblock lang:perl title:perl for迴圈寫法 %}
#陣列印出
my @products = ("foo1", "foo2", "foo3", "foo4", "foo5");
for $item (@prodcts)
{
	print $item;
}
#1印到10
for(1..10){
   print $_;
}
{% endcodeblock %}
#####foreach寫法
{% codeblock lang:perl title:perl foreach迴圈寫法 %}
my @products = ("foo1", "foo2", "foo3", "foo4", "foo5");
foreach $item (@prodcts)
{
	print $item;
}
{% endcodeblock %}

另外，perl是有while可以用的。

###環境變數
$ENV shell環境變數。

$ENV{HOME} 代表找環境變數裡頭home這個變數的值。
{% codeblock lang:text %}
#!/usr/bin/perl

print "Home = $ENV{HOME}\n";

foreach $key (keys %ENV)
{
     print "$key\t$ENV{$key}\n";
}
{% endcodeblock %}

####plugin安裝
使用CPAN或是CPANM，在ubuntu上使用系統的apt-get是最保重互相不會出現conflict最好的方式。