---
layout: post
title: '[sybase]在sybase如果設定identity 為0'
date: 2009-01-18 13:23
comments: true
tags: Sybase
---

{% codeblock lang:sql %}
sp_chgattribute table_name, ‘identity_burn_max’, 0, ’1′
{% endcodeblock %}