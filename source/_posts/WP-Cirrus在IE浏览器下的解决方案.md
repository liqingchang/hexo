---
title: WP-Cirrus在IE浏览器下的解决方案
tags:
  - 3D标签云
  - IE
  - WP-Cirrus
id: 115
categories:
  - Wordpress
date: 2011-01-29 15:01:41
---

用WP-Cirrus实现的3D标签云在Chrome下效果很不错，但是在IE浏览器下却是惨不忍睹

在这里奉劝大家放弃IE这样的浏览器，改用FF或者Chrome

言归正传，做出来的Blog是给别人看的，毕竟我们不能让所有访客都用Chrome，所以只好想想办法解决WP-Cirrus在IE浏览器下的显示

当然，以我这样的水平要改核心代码让3D效果在IE下显示正常是十分遥远的

所以路线决定为在IE浏览器下面不显示3D效果，改为显示正常的普通的标签云

* * *
于是，首先我们要知道怎样确认访客的浏览器

代码

<div style="background:#fdfdfd;color:black;"><u>PHP语言</u>: [临时自用代码](http://fayaa.com/code/view//)</div>
     <div class="source" style="font-family: '[object HTMLOptionElement]', Consolas, 'Lucida Console', 'Courier New'; color: rgb(0, 0, 0); "> <span style="color: rgb(0, 0, 0); ">//正值表达式比对解析$_SERVER[&#39;HTTP_USER_AGENT&#39;]中的字符串 获取访问用户的浏览器的信息 </span>
 <span style="color: rgb(0, 0, 0); ">//判断如果浏览器是IE就返回true </span>
 <span style="color: rgb(0, 0, 0); ">function determinebrowser ($Agent) { </span>
 <span style="color: rgb(0, 0, 0); ">if (ereg(&#39;MSIE ([0-9].[0-9]{1,2})&#39;,$Agent,$version)) { </span>
 <span style="color: rgb(0, 0, 0); ">&nbsp;&nbsp;&nbsp; return true; </span>
 <span style="color: rgb(0, 0, 0); ">} </span>
 <span style="color: rgb(0, 0, 0); ">else { </span>
 <span style="color: rgb(0, 0, 0); ">return false; </span>
 <span style="color: rgb(0, 0, 0); ">} </span>
 <span style="color: rgb(0, 0, 0); ">}</span>
</div>

传入$_SERVER['HTTP_USER_AGENT']给函数determinebrowser，函数正则表达式判断浏览器是否IE浏览器，如果是就返回true，不是就返回false

函数有了，我们现在要修改WP-Cirrus插件的代码，找到下面的代码

 <div style="background:#fdfdfd;color:black;"><u>PHP语言</u>: [Codee#16616](http://fayaa.com/code/view/16616/)</div>
     <div class="source" style="font-family: '[object HTMLOptionElement]', Consolas, 'Lucida Console', 'Courier New'; color: rgb(0, 0, 0); "> <span style="color: rgb(0, 0, 0); ">function wpcirrusWidgetInit($args){</span>
 <span style="color: rgb(0, 0, 0); ">extract($args);</span>
 <span style="color: rgb(0, 0, 0); ">$options = get_option(&#39;wpcirrus-widget&#39;);</span>

 <span style="color: rgb(0, 0, 0); ">echo $before_widget . $before_title . $options[&#39;title&#39;] . $after_title;</span>

 <span style="color: rgb(0, 0, 0); ">wpcirrusInit(false, $args);</span>

 <span style="color: rgb(0, 0, 0); ">echo $after_widget;</span>

 <span style="color: rgb(0, 0, 0); ">}</span>
</div>

这是控制widget在前台显示的代码，我们只需要增加相应的条件判断

<div style="background:#fdfdfd;color:black;"><u>PHP语言</u>: [临时自用代码](http://fayaa.com/code/view//)</div>
     <div class="source" style="font-family: '[object HTMLOptionElement]', Consolas, 'Lucida Console', 'Courier New'; color: rgb(0, 0, 0); "> <span style="color: rgb(0, 0, 0); ">function wpcirrusWidgetInit($args){</span>
 <span style="color: rgb(0, 0, 0); ">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; extract($args);</span>
 <span style="color: rgb(0, 0, 0); ">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $options = get_option(&#39;wpcirrus-widget&#39;);</span>
 <span style="color: rgb(0, 0, 0); ">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; echo $before_widget . $before_title . $options[&#39;title&#39;] . $after_title;</span>
 <span style="color: rgb(0, 0, 0); ">&nbsp; //判断是否IE浏览器</span>
 <span style="color: rgb(0, 0, 0); ">&nbsp; if (!determinebrowser($_SERVER[&#39;HTTP_USER_AGENT&#39;])){</span>
 <span style="color: rgb(0, 0, 0); ">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; wpcirrusInit(false, $args);</span>
 <span style="color: rgb(0, 0, 0); ">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }</span>
 <span style="color: rgb(0, 0, 0); ">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; else{//是IE浏览器，显示普通的标签云&nbsp;&nbsp; </span>
 <span style="color: rgb(0, 0, 0); ">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; echo &#39;&lt;div&gt;&#39;;</span>
 <span style="color: rgb(0, 0, 0); ">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; wp_tag_cloud(&nbsp; );</span>
 <span style="color: rgb(0, 0, 0); ">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; echo &quot;&lt;/div&gt;n&quot;;&nbsp;&nbsp;&nbsp; </span>
 <span style="color: rgb(0, 0, 0); ">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }</span>
 <span style="color: rgb(0, 0, 0); ">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; echo $after_widget;</span>
 <span style="color: rgb(0, 0, 0); ">}</span>
</div>

这样，如果访客用的是IE浏览器访问，WP-Cirrus就只会显示普通的标签云了

很简单，也没什么技术含量，虽然不是完美的解决方案，凑合着用吧

其实我觉得普通的标签云增加彩色比3D标签云更好看