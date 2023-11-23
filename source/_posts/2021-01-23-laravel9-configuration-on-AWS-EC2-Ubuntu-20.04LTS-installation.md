---
layout: post
title: Laravel Configuration on AWS EC2 Ubuntu 20.04LTS Instance Installation - Laravel 9於AWS EC2上的環境設定
date: 2021-01-22 11:39:17
tags: Laravel, PHP
---

有點久沒做環境設定，最近在寫side project遇到了環境設定。即然Laravel都已經到了Laravel 9了，是該把一些整合設定留下來...

### 所需Package
* Ubuntu 20.04 LTS and update
* Curl
* Docker (Laravel 9官方採用Sail這套工具進行環境管理)
* PHP 8.1
* Composer

<!--more-->

### 安裝步驟
#### Step 1 Start a AWS EC2 Instance


Basically, Curl is default installed.

#### Step 2 安裝Docker community version
先確認先前的安裝是否有移除：
```
 sudo apt-get remove docker docker-engine docker.io containerd runc
```
接著更新一下系統
```
sudo apt-get install ca-certificates curl gnupg lsb-release
```
將docker的官方key加入到系統內
```
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
設定使用stable branch的docker
```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
再利用`sudo apt-get update` 更新一下系統，然後來正式安裝了：
```
apt-get install docker-ce docker-ce-cli containerd.io
```
執行以下指令確定是否有安裝成功：
```
sudo docker run hello-world
```

接下來我們會遇到的事情是，因為安裝後要執行docker的相關指令或許會碰到必需要使用root權限(即使用sudo)才能正常執行，但並不想要這麼麻煩，所以接下來需要執行以下script

```
sudo usermod -aG docker ${USER}
su - ${USER}
sudo usermod -aG docker {Username}
```
然後重新打開terminal之後，應該能夠正常的直接使用docker執行不需要再使用sudo來輔助執行了！


Reference: 
* [Docker install](https://docs.docker.com/engine/install/ubuntu/)
* [Digital Ocean: Docker install](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04)

#### Step 3 Install PHP 8.1 on Ubuntu 20.04 LTS
先安裝以下套件：
```
sudo apt install software-properties-common
```
接下來加入以下PHP套件來源
```
sudo add-apt-repository ppa:ondrej/php
```
接著進行update
```
sudo apt update
```
正式安裝php所需的相關套件：(請依照所需進行安裝)
```
sudo apt-get install php8.1 php8.1-common php8.1-mbstring php8.1-mcrypt php8.1-cli php8.1-fpm php8.1-mysql php8.1-zip
```

接著就可以在shell中試著顯示PHP版本確認是否安裝成功~


#### Step 4 Install Composer
請參照Composer官方網站執行以下script
```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '906a84df04cea2aa72f40b5f787e49f22d4c2f19492ac310e8cba5b96ac8b64115ac402c8cd292b8a03482574915d1a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
```
執行後，在本機執行ls指令會看到 `composer.phar` 這個檔案，為了日後執行方便，請執行以下script:

```
sudo mv composer.phar /usr/local/bin/composer

```
成功執行之後，就可以在shell直接以下列指令直接執行composer工作
```
composer
```


----------
接下來你可以clone your repository project或是開始進行Laravel 建立新專案進行開發作業。
