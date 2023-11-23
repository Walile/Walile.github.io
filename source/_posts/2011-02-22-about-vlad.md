---
layout: post
title: '[vlad] vlad心得筆記'
date: 2011-02-22 06:08
comments: true
tags: Vlad
---

號稱跟capistrano很像，但是較不複雜。
<!--more-->

##安裝：

1. ruby (目前我用v1.8)
2. gem tool

安裝必要套件：
Rake
open4
hoe
rubyforge

再用gem tool安裝vlad

{% codeblock lang:text %}	
gem install vlad
{% endcodeblock %}

##操作：

###一、新增一個專案的時候，需要先更改一下Rakefile，新增以下二行：

{% codeblock lang:text %}	
require ‘vlad’
Vlad.load
{% endcodeblock %}

###二、修改config/deploy.rb
將需要設定的變數設定進去~ 如：

{% codeblock lang:text %}	
set :application, "project2″

set :user, "doris"
set :domain, "192.168.1.104″
set :deploy_to, "/home/doris/Documents/project2/"
set :repository, "http://info.wahaha.com/svn/project/trunk/"
set :svn_cmd, ‘svn –username username –password "password"‘
{% endcodeblock %}

###三、執行安裝時，直接輸入：

{% codeblock lang:text %}	
rake vlad:setup vlad:update
{% endcodeblock %}
(第一次需要這樣執行)

就會進行新專案佈署。其中，vlad:setup只是將server上新建folder，並且長的跟repository的一樣。所以要做vlad:update檔案才會寫進去。

執行multi-task的時候：

####1. multi-task寫法如下：
{% codeblock lang:ruby %}	
task :dep do
    puts "abc"
end
{% endcodeblock %}
執行的時候，語法如下：

{% codeblock lang:text %}	
rake dep
{% endcodeblock %}

就會直接執行task為dep這個task

####2. 執行rake vlad:setup beta 會執行預設的setup後，直接再執行beta這個task

####3. 執行一個task後，要去執行第二個task
example:

{% codeblock lang:ruby %}	
task :task1 do
    puts "hello"
end

task :task2 do
    puts "do this"
end

task :task1 => :task2
{% endcodeblock %}

當執行了rake task1之後，會先執行dodo這個task2後，才會執行task1。

####4. multi-config和capistrano一樣，需要設定，以task分成production以及dev，範例如下：

{% codeblock lang:ruby %}	
task :production
set :deploy_to, "/home/doris/Documents/project"
puts "#{deploy_to}"
end

task :dev
set :deploy_to, "/home/doris/Documents/dev"
puts "#{deploy_to}"
end

{% endcodeblock %}

這樣執行rake production的時候，會去執行production的設定，而會印出production的deploy_to的位置。#{變數名稱} 可是用來印出變數用

####5. 似乎是不能像capistrano有跨task呼叫的功能。

####6. default預設就是用svn

####7. 也可用capistrano的run來執行command line指令

####8. 進行update請用rake vlad:update，要做rollback請用rake vlad:rollback

和Capistrano不同的地方:
-----

程式進行update(vlad:update)、restart、進行資料庫更新(vlad:migrate)、伺服器啟動(vlad:start)都以執令執行。

deploy.rb直接建立檔案處理，capistrano利用指令建立deploy.rb以及產生capfile處理。Vlad從此看的出來需要倚靠Rake做deploy的動作。

部分deploy.rb的變數不太一樣。

vlad沒有after、before這二個function，因為是用rake

php做deployment:
------

1. copy rails專案中的Rakefile到目前專案的位置
2. 以及configs底下的boot.rb、environment.rb到專案的configs底下。
3. 新建deploy.rb進行建置任務的動作

vlad可使用的變數可看[這邊](http://hitsquad.rubyforge.org/vlad/doco/variables_txt.html)
