---
layout: post
title: '[coffeScript] coffee script 設定安裝'
date: 2011-12-16 06:49
comments: true
tags: CoffeScript
---

因為開發平台是使用mac，所以設定的部分將以mac為準。
<!--more-->

###設定: 

#####環境：
1. mac os 10.7.2 (Lion)
2. homebrew

先安裝好 homebrew，homebew的安裝法可參考: ihower的設定方法

接下來，先來安裝Node.js
{% codeblock lang:text %}
brew install node
{% endcodeblock %}

再來安裝npm (node package manager)
{% codeblock lang:text %}
curl http://npmjs.org/install.sh | sh
{% endcodeblock %}

接下來安裝coffee script
{% codeblock lang:text %}
npm install -g coffee-script
{% endcodeblock %}

將node_modules資料夾加到路徑之中
{% codeblock lang:text %}
export NODE_PATH=/usr/local/lib/node_modules
{% endcodeblock %}

當設定完成之後，可以直接按下
{% codeblock lang:text %}
node
{% endcodeblock %}

此時，會在畫面上看到進入了一個可以鍵入程式的環境，並且是以>開頭的畫面。接下來只要鍵入：
{% codeblock lang:text %}
require('coffee-script')
{% endcodeblock %}

如果出現了像以下的訊息
{% codeblock lang:coffeescript %}
{ VERSION: '1.1.3',
  RESERVED: 
   [ 'case',
     'default',
     'function',
     'var',
     'void',
     'with',
     'const',
     'let',
     'enum',
     'export',
     'import',
     'native',
     '__hasProp',
     '__extends',
     '__slice',
     '__bind',
     '__indexOf',
     'true',
     'false',
     'null',
     'this',
     'new',
     'delete',
     'typeof',
     'in',
     'instanceof',
     'return',
     'throw',
     'break',
     'continue',
     'debugger',
     'if',
     'else',
     'switch',
     'for',
     'while',
     'do',
     'try',
     'catch',
     'finally',
     'class',
     'extends',
     'super',
     'undefined',
     'then',
     'unless',
     'until',
     'loop',
     'of',
     'by',
     'when',
     'and',
     'or',
     'is',
     'isnt',
     'not',
     'yes',
     'no',
     'on',
     'off' ],
  helpers: 
   { starts: [Function],
     ends: [Function],
     compact: [Function],
     count: [Function],
     merge: [Function],
     extend: [Function],
     flatten: [Function],
     del: [Function],
     last: [Function] },
  compile: [Function],
  tokens: [Function],
  nodes: [Function],
  run: [Function],
  eval: [Function] }
{% endcodeblock %}
代表設定成功了。要退出目前的狀態只要<code>Control C</code>按個二下就行了。
