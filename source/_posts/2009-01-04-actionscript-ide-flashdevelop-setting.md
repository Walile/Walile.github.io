---
layout: post
title: '[Actionscript] 好用的IDE: flashdevelop設定 (only for windows)'
date: 2009-01-04 13:16
comments: true
tags: ActionScript
---

預先記得先裝好.Net Framework2.0，，因為flashdevelop是用.net寫成的。
此外，因為這個IDE已經整合好了，所以不用裝ant，但是記的要裝java SDK。
<!--more-->
1. 先到FlashDevelop.org下載版本，目前我是用3.0.0beta 9。

2. 到adobe的網站上下載flex的SDK(是SDK，別下載錯了)目前是用flex3的版本。

3. 下載flash10的player 一樣在adobe的網站上找的到。

4. 在flashdevelop的選單中的「Tools/setting」選擇AS3Context_plugin的地方，將flex SDK location中設定自己本機上放置flex sdk的位置。

5. 再來設定flash preview的地方。接下來選擇「FlashViewer plugin」的選項，然後將「External Player Location」設為放置flash player 10的路徑，並且將Movie display type的地方更改為External.

6. 接下來，開一個檔案，將語法設定為AS3，然後輸入以下建立一個hello world的程式！
{% codeblock 比較 == lang:php %}
package {
    import flash.display.*;
    import flash.events.*;

    public class Test extends Sprite {

        public function Test() {

        }

    }
}
{% endcodeblock %}
如果按ctrl+F8後有成功編譯，就是設定成功了