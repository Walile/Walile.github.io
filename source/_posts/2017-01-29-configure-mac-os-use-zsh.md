---
layout: post
title: 設定zsh為預設shell (linux平台)
date: 2017-01-29 11:12:34
tags: R-language
---
其實用了超久結果每次電腦重灌都要再挖一次記錄，直接簡單記錄改用zsh做為基本使用的shell。
<!--more-->
首先，要先裝好[homebrew](http://brew.sh/index_zh-tw.html):

1. brew install zsh
2. 設定oh-my-zsh: 以下擇一即可，一個是用wget來安裝，另一個是curl來處理
	- sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
  - sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"


(設定使用[https://github.com/robbyrussell/oh-my-zsh](oh-my-zsh))
3. brew install zsh-completions

基本上應該這樣就可以完成基本設定，而且預設會把bash改成zsh。另外，如果想修改theme的話，預設系統的theme是放在：~/.oh-my-zsh/themes (theme是由oh-my-zsh提供的)，基本上來說，可以透過這篇[官方說明](https://github.com/robbyrussell/oh-my-zsh/wiki/Themes)來設定。這樣就大功告成了。