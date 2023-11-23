---
title: 在mac上建立python開發的虛擬環境virtualenv
date: 2016-03-16 00:42:24
tags: Python
---
python virtualenv configuration。(參照：[virtualenv官網](https://virtualenv.pypa.io/en/latest/installation.html))

目前環境：Mac OS 10.11.3 (EI capitan)
原生Python Version: 2.7.10

<!--more-->
## Python環境設定：
1. 先在mac環境上已經有原生的Python，版本是2.7.10。接下來先執行以下指令安裝pip
	
	sudo easy_install pip
	
2. 安裝 virtualenv:
安裝pip之後，執行pip來安裝virtualenv

	pip install virtualenv

3. 接下來，可以直接執行

	virtualenv python27 # python27為資料夾的名稱
	
這樣會安裝到指定的資料夾名稱之內。要使用這個virtual environment的時候，只要：

	source python27/bin/active

就可以執行這個環境。要離開的時候，也只需要直接執行

	deactive
	
就可以退出這個virutualenv了。