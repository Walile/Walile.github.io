---
layout: post
title: 'spring2 and velocity implement i18n'
date: 2010-06-08 03:45
comments: true
tags: Java Spring
---

這次手上的專案，因為使用了Hiberanet + Spring 2.x mvc + Velocity 1.6.3結合組成。處理i18n的時候，其實有點不知道該怎麼做。因為純Spring處理時，可以直接利用內建的spring mvc viewer tag去標示在view之中。只是這次view的部分使用了Velocity，於是必需要想一下該怎樣才能將訊息正確的出現在view的部分。
<!--more-->
{% codeblock lang:xml %}
<bean id="velocityConfig" class="org.springframework.web.servlet.view.velocity.VelocityConfigurer">
<property name="resourceLoaderPath" value="/" />
</bean>
{% endcodeblock%}

其實這段設定，只是在設定view的地方使用Velocity。接下來要來設定在applicationContext中的細部設定。

{% codeblock lang:xml %}
<bean id="messageSource">
<property name="basename" value="message" />
<property name="useCodeAsDefaultMessage" value="true" />
</bean>
{% endcodeblock %}

basename的部分是在設定properties的檔案prefix的部分。接下來想設定可讀取message_en_us.properties以及message_zh_tw.properties二個檔案。其中前頭為「message」開頭，所以basename的value則設定為message。此外，另外在useCodeAsDefaultMessage的地方設定預設語言的部分設定為true。

{% codeblock lang:xml %}
<bean id="localeChangeInterceptor">
<property name="paramName" value="lang" />
</bean>
{% endcodeblock %}

在localeChangeInterceptor的地方，property可以設定利用那個參數來接收設定語系參數值。

{% codeblock lang:xml %}
<bean id="defaultUrlMapping">
<property name="interceptors" ref="localeChangeInterceptor" />
<property name="order">
<value>1</value>
</property>
</bean>
{% endcodeblock %}

因為我有設定defaultUrlMapping，所以只要設定URL參數有lang=語系，就會讀取相關的properties檔案。

Velocity的部分，就使用#springMessage(messageCode) or #springMessageText(messageCode, defaultText).這二個method套用在view端將資訊顯示出來即可。

如果在處理過程中，出現如"Cannot change HTTP accept header - use a different locale resolution strategy"的訊息告知，則代表原Spring source code中有限制。所以需要改寫方法處理。這時需要繼承org.springframework.web.servlet.i18n.AcceptHeaderLocaleResolver做改寫setLocale的動作。代碼如下：

{% codeblock lang:java %}
package model.data;

import org.springframework.web.servlet.i18n.AcceptHeaderLocaleResolver;
import java.util.Locale;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class NewAcceptHeaderLocaleResolver extends AcceptHeaderLocaleResolver {

    private Locale mylocal;

    public Locale resolveLocale(HttpServletRequest request){
        return mylocal;
    }

    public void setLocale(HttpServletRequest request, HttpServletResponse response, Locale locale){
        mylocal = locale;
    }
}
{% endcodeblock %}

接著，需要在applicationContext的檔案中，多加個bean....

{% codeblock lang:xml %}
<bean id="localeResolver" class="model.data.NewAcceptHeaderLocaleResolver" />
{% endcodeblock %}

接下來就不會出現如之前所說的錯誤了。