---
layout: post
title: '[mercurial] hg 設定'
date: 2009-12-17 02:59
comments: true
tags: Mercurial
---

<!--more-->
{% codeblock lang:python %}
[web]
push_ssl = false
allow_push = *
{% endcodeblock %}
因為預設hg新建資源庫的時候，會用ssl potocal
所以如果不想用，可以在repo中的./svn這個資料夾中，找hgrc這個檔案(如果沒有新增一個)，然後加上上頭的資訊。
「allow_push = *」設定為「*」為表示大家都可以進行存取該資源庫的動作，不需要經過認證。如果有需要認證的話，這個部分需要再做設定。