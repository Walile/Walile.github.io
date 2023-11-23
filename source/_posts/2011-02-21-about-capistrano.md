---
layout: post
title: '[capistrano]capistrano心得筆記'
date: 2011-02-21 05:57
comments: true
tags: Capistrano
---

這是deployment的工具，而且是ruby的。另有Vlad，也是ruby的deploy工具。
<!--more-->

##一、安裝：

1. ruby (目前我用v1.8)
2. gem tool

再用gem tool安裝capistrano
{% codeblock lang:text %}	
1. gem sources -a http://gems.github.com/  #(加source)
2. gem install capistrano
{% endcodeblock %}

安裝成功時，可以在command line下直接使用cap指令

##二、開始使用
1. 新建專案後，先進入專案資料夾，接著輸入

{% codeblock lang:text %}
capify .
{% endcodeblock %}

執行後會在rails的資料夾中產生以下二個檔案

{% codeblock lang:text %}
./config/deploy.rb
./Capfile
{% endcodeblock %}

這樣就可以開始改deploy.rb了！

心得重點：
-------
設定資料需寫在deploy.rb之中，執行的動作也以ruby寫出。

基本執行的時候，會直接將deploy.rb設定的資料直接做處理動作。

執行cap deploy的時候，其deploy不是指檔案名稱，是deploy預設的動作。這個不會出現在deploy.rb的設定中。但可以利用撰寫同名的namespace來設定想要動作的任務。

執行設定主機有localhost會有問題，因為是遠端佈署用工具。都是利用ssh之類做操作。

在執行前，需要先remote server的public key，在做ssh 的時候會比較方便些。

直接在deploy.rb中寫好密碼，密碼有出現符號的部分，最好前、後加上跳脫符號，系統才會認得。

可以自行寫task進行執行，方法如下：

{% codeblock lang:ruby %}
desc "check out project"
task :check_out_project do
    puts "check out project"
end
{% endcodeblock %}

在desc 後，寫下task的名稱(只是標示用)，然後task後寫出這個task的名稱，但是這個名稱是到時候執行的時候會認的參數，如上述的程式碼，要執行這段task時，在提示符號下，就需要打上：cap check_out_project 這樣才會執行在這個task撰寫的動作。(上述程式碼上，就會印出check out project)。

如果需要出現二個namespace的區塊，用來放置不同動作分類task時，範例如下：

{% codeblock lang:ruby %}
namespace :checkout do
    task :default, :roles => :web do
        puts "check default"
        checkout.callbackup
    end
    task :callbackup do
        backup.default
    end
end

namespace :backup do
    task :default, :roles => :web do
        puts "backup default"
    end
    task :backup_project do
        puts "backup project"
    end
end

after(:checkout, "backup:backup_project")

{% endcodeblock %}

1. 這樣可以在提示字元下，另用參數來切換要使用那一個指令，如：

{% codeblock lang:ruby %}
cap checkout:default #這個代表用來執行在checkout這個namespace之中default的task
cap backup:default #這個代表用來執行在backup這個namespace之中default的task
{% endcodeblock %}

此外，task可以跨task、namespace呼叫，方法就是在task之中，依上述範例，可寫下如「back.default」，這代表呼叫backup這個namespace中的default這個task。而同namespace的其他task呼叫，依上述範例，在checkout的default這個task之中，只要寫下如「checkout.callbackup」，這代表呼叫checkout的callbackup這個task。

2. 當程式出現了after()這個function時，這代表可以指定那個namespace的task只要執行完之後，就會去執行指定的另個task。如after(:checkout, "backup:backup_project") 的意思是，當執行了checkout這個namespace之中的task之後，就會去執行backup這個namespace之中的backup_project這個task。

3. after("deploy", "backup:backup_project")
可以指定如果使用deploy的時候，在執行後會去跑backup這個namespace的backup_project這個task，不過在實作中，deploy需要用namespace再撰寫一次，指定在deploy時會處理的動作。這樣在執行cap deploy的時候，才會在執行後觸發backup的backup_project。

4. 在task之中如果想下系統指令：
{% codeblock lang:text %}
run "ls"
{% endcodeblock %}
只要如上述將指令寫在run 的後面就可以了。
這個執行是執行server上的執令

5. 設定變數
在capistrano中，只要設定好參數，即可帶參數在task之中出現，如：
set :username, ‘myName’
然後，在task程式如下撰寫：
{% codeblock lang:ruby %}
task :default
    puts "hello #{username}"
end
{% endcodeblock %}

在執行這個task的時候，就會印出hello myName
(#{變數名稱} 會替換成所設定的變數的值)

6. 想在deploy中分開production、stagging環境：
在deploy.rb中，只需要寫：

{% codeblock lang:ruby %}
task :production do
  set :deploy_to, "/u/apps/#{application}-production/"
  set :deploy_via, :remote_cache
  after('deploy:symlink', 'cache:clear')
end

task :staging do
  set :deploy_to, "/u/apps/#{application}-staging/"
  set :deploy_via, :copy
  after('deploy:symlink', 'cruise_control:build')
end
{% endcodeblock %}
在下指令的時候，只需要像
{% codeblock lang:text %}
cap production deploy
cap staging deploy
{% endcodeblock %}

就可以分環境進行任務。

而capistrano針對php做deploy的作法：

1. 進入專案資料夾後，直接執行：

{% codeblock lang:text %}
capify .
{% endcodeblock %}

然後，在專案資料夾下，會產生一個叫Capfile的檔案，還有在config資料夾裡有一個deploy.rb的檔案。

2. 只要編輯好deploy.rb這個檔案即可以進行動作。