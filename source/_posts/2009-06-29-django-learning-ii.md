---
layout: post
title: '[Django] 入門學習 II'
date: 2009-06-29 16:08
comments: true
tags: Django
---

開始來準備建立模組(models)了。
<!--more-->
但是在此之前，有需要解譯一下apps和project不同的地方，簡言之，project專案中會含蓋很多apps。其實意思是很多的application會組成一個專案。那application就像是一個後台功能、一個weblog系統之類的。這應該是比較好懂的解譯法。知道這二者的不同之處後，接下來要做的，就是要來建立一個app了。

接下來要建立的app將在之前的blog專案中建立。所以請先確認目前自己下指令的位置是不是在上一篇先建立的blog專案的路徑中。(在本範例中是在d:pyblog 中)如果沒有的話，請先進入到blog專案的位置。(如果還沒建立blog專案，請先參照此文進行建立專案的動作後，再回來繼續下去。)然後，接下來建入以下的指令：

{% codeblock lang:python %}
python manage.py startapp polls
{% endcodeblock %}

輸入執行後，會發現在原來的資料夾中，多了一個叫做「polls」的資料夾，其中也出現了三個檔案：「models.py 、views.py 、 __init__.py」下一步開始，要來建立這個polls的相關功能還有資料表了。

建立資料表在Django可不是直接去資料庫去建上一個，而是利用本身Django的ORM的機制做來。怎麼做呢？來寫個class就可以了。現在我們打開在polls資料夾一個叫做models.py的檔案，並且將以下的程式寫入：

{% codeblock lang:python %}
from django.db import models

class Poll(models.Model):

question = models.CharField(max_length=200)

pub_date = models.DateTimeField(‘date plublished’)

class Choice(models.Model):

poll = models.ForeignKey(Poll)

choice = models.CharField(max_length=200)

votes = models.IntegerField()
{% endcodeblock %}
註：max_length需要最新版本的Django才不會出錯！

在以上的程式中所定義的，是這個polls app所需要的資料表及欄位設定(像是data type或是欄位長度、那個是ForeignKey)。在這邊定義了二個table。詳細設定請再參照Django document。

關於Model欄位設定相關資料類型的部分請參照[這邊](http://docs.djangoproject.com/en/dev/ref/models/fields/#django.db.models.Field.blank)


當將models.py程式寫好後。請記得存檔。

開始準備要進行啟動模組的動作了。在啟動的過程式將會把上文所設定的database schema一併開好。

在啟動模組前，先做一個設定上的動作。將polls這個模組放進去。所以，請先打開blogs資料夾中的settings.py這個檔案，然後找到INSTALLED_APPS的地方進行編輯：

{% codeblock lang:python %}

INSTALLED_APPS = (

‘django.contrib.auth’,

‘django.contrib.contenttypes’,

‘django.contrib.sessions’,

‘django.contrib.sites’,

‘django.contrib.admin’,

‘blog.polls’
}

{% endcodeblock %}

紅色的地方是自己需要加上去的。是自己目前新增的功能。目前加的是blogs專案的polls功能，所以就寫上「blogs.polls」(記得要加單引號代表為字串)這個意思是讓這個系統有polls這個功能。

接下來在blog資料夾執行以下指令：(不要進polls資料夾)

{% codeblock lang:python %}
python manage.py sql polls
{% endcodeblock %}

當指令執行成功，會出現以下的訊息：

{% codeblock lang:sql %}
BEGIN;

CREATE TABLE "polls_poll" (

"id" integer NOT NULL PRIMARY KEY,

"question" varchar(200) NOT NULL,

"pub_date" datetime NOT NULL

)

;

CREATE TABLE "polls_choice" (

"id" integer NOT NULL PRIMARY KEY,

"poll_id" integer NOT NULL REFERENCES "polls_poll" ("id"),

"choice" varchar(200) NOT NULL,

"votes" integer NOT NULL

)

;

COMMIT;
{% endcodeblock %}

其實這個就是表示成功產生polls這個app所要使用的table的sql。而這些sql其實就是之前寫程式設定的table還有欄位的設定sql。接著，要真正的在資料庫中建立這個app的table，請下下面的指令執行：

{% codeblock lang:python %}
python manage.py syncdb
{% endcodeblock %}

如果沒有看到錯誤訊息，這樣就完成了。
此外，關於table的查詢，可以透過[database api document](http://docs.djangoproject.com/en/dev/topics/db/queries/#topics-db-queries)這篇文章來使用。

後台開始要來設定了。首先打開blog資料夾中的settings.py，將後台的管理系統加入。一樣是加在「INSTALLED_APPS」之中。

{% codeblock lang:python %}
INSTALLED_APPS = (
‘django.contrib.auth’,
‘django.contrib.contenttypes’,
‘django.contrib.sessions’,
‘django.contrib.sites’,
‘blog.polls’,
‘django.contrib.admin’
)
{% endcodeblock %}

接著，一樣來下個指立建立一下db吧

{% codeblock lang:python %}
python manage.py syncdb
{% endcodeblock %}

接下來要編輯blog資料夾中的urls.py。然後將第4,5,16行的註解拿掉。程式就會長得像下面的樣子：

{% codeblock lang:python %}
rom django.conf.urls.defaults import *

Uncomment the next two lines to enable the admin:
from django.contrib import admin
admin.autodiscover()

urlpatterns = patterns('',
# Example:
# (r'^blog/', include('blog.foo.urls')),

# Uncomment the admin/doc line below and add 'django.contrib.admindocs'
# to INSTALLED_APPS to enable admin documentation:
# (r'^admin/doc/', include('django.contrib.admindocs.urls')),
# Uncomment the next line to enable the admin:
(r'^admin/(.*)', admin.site.root),
)
{% endcodeblock%}

現在可以來看看目前網站的樣子

接下來執行一下…
{% codeblock lang:python %}
python manage.py runserver
{% endcodeblock %}

執行後，直接連過去看(目前我是跑在本機，所以網址是http://localhost:8000/admin/)我們就可以看到網站管理的後台登入畫面出現了！請登入！(可是有中文的哩)英文版的登入後會出現下面的畫面:
![Alt django admin](http://docs.djangoproject.com/en/dev/_images/admin02t.png "django 後台畫面(官方英文圖片)")

現在在畫面上還看不到之前我們寫的Polls這個app，所以我們要做一個動作將我們寫的polls註冊進入這個管理系統。在blog資料夾裡頭polls這個資料夾建立一個叫做admin.py的檔案。並且鍵入下面的程式碼：

{% codeblock lang:python %}
from blog.polls.models import Poll
from django.contrib import admin
admin.site.register(Poll)
{% endcodeblock %}

接下來只要重新啟動server，就可以在畫面上看到polls已經被加入到系統裡面了。
![Alt django admin](http://docs.djangoproject.com/en/dev/_images/admin03t.png "django 後台加入polls的畫面(官方英文圖片)")

當你點了Polls進去後，會看到可以新加post，一切都是自動產生出來的功能。當然，已經有新增的項目也可以做修改的動作。

下一步開始，將要來客制化這個表單。