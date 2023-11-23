---
layout: post
title: 'Perl 入門轉換 part 2'
date: 2014-05-10 05:37
comments: true
tags: Perl
---
這篇來些進階的...檔案IO以及正規表示法的部分
<!--more-->
### 檔案輸出入 (File I/O)
可以根據使用「>」或「<」來指定是否是讀檔或是輸出檔案。
「<」是讀入，而「>」是輸出 (這點跟shell script是一致的)
另外會看到「**<>**」這個符號，一般稱之為**鑽石符號**。這符號用途是將一個屬於陣列夾在鑽石符號中，然後將其值一個一個讀取出來。(有人會認為那是一種iterator...這點我也需要求證。但感覺是蠻像的)
通常
#####一般讀檔並印出內容
{% codeblock lang:perl title: read file and print it %}
my $file = "list.txt";

if(open(FH, '<', $file)){
    while(<FH>){
        print $_;
    }
    close(FH);
}
# 這樣會把list.txt的檔案內容讀出來
{% endcodeblock %}
##### 指定輸入編碼，並讀檔後印出內容
{% codeblock lang:perl title: read file by encoding utf8 and print it %}
my $file = "list.txt";

if(open(FH, '<:utf8', $file)){
    while(<FH>){
        print $_;
    }
    close(FH);
}
# 這樣會把list.txt的檔案內容用UTF8讀出來。然後印出
{% endcodeblock %}

##### 輸出至檔案
{% codeblock lang:perl title: read file and output to a file %}
my $file = "list.txt";

if(open(FH, '<', $file)){
    if(open (FILE, '>', "output.txt")){
        while(<FH>){
            print FILE $_;
        }
    }

    close(FILE);
    close(FH);
}
# 這會將list.txt的內容，完整的輸出到output.txt檔案之中
{% endcodeblock %}

其實照範例來看，perl的語法上其實簡單不少。但因為一些個人學習上的經驗，反而需要去多瞭解清楚背後的意義。否則，其實是很好懂的語法。(人工混碼不算啊 ~ ~ 啊 ~ ~ 啊 ~ ~)

### 正規表示法 (Reqular Expression)
其實在很多語言裡都有正規表示法。在Perl之中，正規表示法是Perl的第二把超級利器。而我也發現在Perl裡寫reqular expression的機率大概高於別的語言不少。其實正式表示法真的寫起來是蠻大一篇的。我這邊推薦可以直接閱讀以下二個連結：
- [Perl 學習手札](http://perl.hcchien.org/TOC.html "Perl 學習手札")
- [Perl的基本語法](http://ind.ntou.edu.tw/~dada/cgi/Perlsynx.htm "Perl的基本語法")

這二個連結其實在學習Perl上會有很大的幫助。

##### 從match pattern中取得變數
在Perl的RE中，可以在表示法中設定要取得的位置為變數，接下來程式可以拿來處理。例如：
{% codeblock lang:perl title: get parameter from RE pattern match %}
use strict;

my $content = "Perl is Great!";
if($content =~ /^(\w*) \w* (\w*)!/){ # 一個括號代表會有一個變數。照左至右取得順序。
    print $1; # 預設的變數名稱都是由1開始編號下去。直接取得既可
    print $2;
}
# 所以會印出：
# $1是Perl
# $2是Great
{% endcodeblock %}

##### 從match pattern中替換文字
有一件很重要的事：***在Perl中，要實現replace(取代)文字的功能，是需要利用RE來處理的。所以一定要會！***
在下面範例示範怎樣取代原來的文字

{% codeblock lang:perl Title: replace word by RE pattern match %}
my $content = "Perl is Great!";
if($content =~ s/Perl/Java/gi){ 
#開頭s是表示要做替換。而結尾的gi修飾子，g是代表內容全部都要搜尋，i比對的時候不需要在意大小寫。
    print $content;
}
# 所以會印出：Java is Great!
{% endcodeblock %}

更多的部分跟屬性，其實參考先前二個很連結，會有很多收獲，接下來就是要好好的練習囉。