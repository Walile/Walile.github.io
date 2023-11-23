---
layout: post
title: '[zend frameowrk]將jQuery plugin - multiselect2side 整合zend_formintegration'
date: 2011-02-21 05:48
comments: true
tags: Zend Framework1
---

<!--more-->

controller之中加入zend_form
{% codeblock lang:php %}	
<?php
$form = new Zend_Form();
        $form->addElement(new Zend_Form_Element_Multiselect('demo1',
                        array('multiOptions' => array(
                                '1' => '選項一',
                                '2' => '選項二',
                                '3' => '選項三',
                                '4' => '選項四',
                                '5' => '選項五',
                                '6' => '選項六',
                                '7' => '選項七',
                                '8' => '選項八',
                            )
                )));
        $this->view->assign('form', $form);
?>
{% endcodeblock %}
在Zend_Form_Element_Multiselect中設定這個項目的名稱(id)為demo1時，其實在html中，就會發現id="demo1″這樣的屬性。所以，在js實作的時候，只需要
{% codeblock lang:javascript %}
$("#demo1″).multiselect2side();
{% endcodeblock %}
就會直接套上了。其他一些選項的部分，只需要直接在function中設定即可，如下：
{% codeblock lang:javascript %}
$("#demo2″).multiselect2side(
{
    moveOptions: false,
}
);
{% endcodeblock %}
上例的程式中，選項當moveOptions設定為false的時候，會將排序的選項功能整個拿掉。