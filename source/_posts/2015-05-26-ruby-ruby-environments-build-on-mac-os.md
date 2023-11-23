---
title: Mac上如何建立Rails的開發環境
date: 2015-09-17 00:42:24
tags: Ruby
---
太久沒玩Ruby很多東西也整個忘的很乾淨了。這次試著將一些東西撿回來(主要是想使用Ruby on Rails)。所以重新整理了環境建置的筆記。(主要的intruction是參照：ihower的[Ruby on Rails 實戰聖經](https://ihower.tw/rails4/advanced-installation.html))

目前環境：Mac OS 10.10 (Yosemite)
原生Ruby Version: 2.0.0p481
<!--more-->
## Ruby環境設定：
1. 先在mac環境上安裝Homebrew (一個方便在mac上使用的工具套裝管理套件。也有的人喜歡用MacPorts) 連結：http://brew.sh/
2. 安裝RVM:
安裝RVM的目的主要是可以指定安裝Ruby/JRuby的版本並進行環境的管理。
    - Instruction: 請見[這邊](https://rvm.io/)
    - 目前版本的RVM安裝前，會需要先處理security的部分。所以會需要安裝gpg(或gpg2等套件)進行才能進行設定。(這時候會需要利用brew install gpg
    - security安裝設定請見：[這邊](https://rvm.io/rvm/security)
    - 完整的RVM官方安裝說明請見：[這邊](https://rvm.io/rvm/install)

3. 請記得安裝 MySQL (這個部分安裝是為了使用Ruby on Rails用的。如果不需要也可以不用安裝。建置可以直接利用homebrew安裝即可。也可以從官方下載。官方下載安裝可以按裝其中附的套件，就可以在「設定」中進行MySQL Server的開啟/關閉)

寫到這邊大致也完成了基本的環境建置。接下來要設定Rails的部分了...

## Rails環境設定：
Ruby環境的前置作業設定後，即可以使用Gem來做Ruby的套件管理。接下來要安裝Rails的環境：
**gem install rails**
接下來會將該套件的相關package下載進行安裝。(如果不需要安裝套件內包裝的文件。請記得加上參數或在預設gem設定加預設安裝即不安裝文件)
目前Gem套件也和Perl一樣有網站可以查看其熱門度。[The Ruby Toolbox](https://www.ruby-toolbox.com/)

到目前為止，大致完成環境設定。就可以進行下一步了。