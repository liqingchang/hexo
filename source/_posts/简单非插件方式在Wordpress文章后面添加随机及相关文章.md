---
title: 简单非插件方式在Wordpress文章后面添加随机及相关文章
tags:
  - HTML
  - PHP
  - 相关文章
  - 随机文章
id: 87
categories:
  - Wordpress
date: 2010-12-22 23:15:05
---

<span style="color: #ffffff;">啊，没地方写自己的东西了，虽然没人来这里，不过还是反白下装装逼吧~写下这个，想在不久或者很久的未来，告诉自己，在这样的日子里，曾经发生过这样的事，改变了你
今天意外地发现原来自己一直对别人造成困扰
整个下午都没什么心思工作，以前老师讲：你怎么对别人，别人也会怎样对你。
这句话，只适用于人的'恶'
几句话回荡在脑海中
深刻地影响着我
生活还是这样过
只是我已经不想再做我了</span>

<span style="color: #000000;"><span style="color: #000000;">有随机/相关文章的好</span>处：对访客更友好，可以激发访客访问其他页面的欲望。</span>

<span style="color: #000000;">方法也很简单，首先找到你的主题的single.php，这就是文章的</span>模板，然后假如你想把随机文章或者相关文章放在评论后面，请在评论前假如以下代码
<div style="background: #fdfdfd; color: black;"><span style="text-decoration: underline;">PHP语言</span>: [Codee#15974](http://fayaa.com/code/view/15974/)</div>
<div class="source" style="font-family: '[object HTMLOptionElement]', Consolas, 'Lucida Console', 'Courier New'; color: #000000;"><span style="color: #008800; font-style: italic;">01 </span> <span style="color: #000000;">&lt;div class="relate hfr"&gt;</span>
<span style="color: #008800; font-style: italic;">02 </span> <span style="color: #000000;">&lt;ul id=”cat_related”&gt;</span>
<span style="color: #008800; font-style: italic;">03 </span> <span style="color: #000000;">Random Articles</span>
<span style="color: #008800; font-style: italic;">04 </span> <span style="color: #008080;">&lt;?php</span>
<span style="color: #f810b0;">05 </span> <span style="color: #000000;">$cats</span> <span style="color: #000000;">=</span> <span style="color: #000000;">wp_get_post_categories</span>(<span style="color: #000000;">$post</span><span style="color: #000000;">-&gt;</span><span style="color: #ff0000;">ID</span>);
<span style="color: #008800; font-style: italic;">06 </span> <span style="color: #000080; font-weight: bold;">if</span> (<span style="color: #000000;">$cats</span>) <span style="color: #000000;">{</span>
<span style="color: #008800; font-style: italic;">07 </span> <span style="color: #000000;">$cat</span> <span style="color: #000000;">=</span> <span style="color: #000000;">get_category</span>( <span style="color: #000000;">$cats</span><span style="color: #000000;">[</span><span style="color: #0000ff;">0</span><span style="color: #000000;">]</span> );
<span style="color: #008800; font-style: italic;">08 </span> <span style="color: #000000;">$first_cat</span> <span style="color: #000000;">=</span> <span style="color: #000000;">$cat</span><span style="color: #000000;">-&gt;</span><span style="color: #ff0000;">cat_ID</span>;
<span style="color: #008800; font-style: italic;">09 </span> <span style="color: #000000;">$args</span> <span style="color: #000000;">=</span> <span style="color: #000080; font-weight: bold;">array</span>(
<span style="color: #f810b0;">10 </span> <span style="color: #0000ff;">'showposts'</span> <span style="color: #000000;">=&gt;</span> <span style="color: #0000ff;">6</span><span style="color: #000000;">,</span>
<span style="color: #008800; font-style: italic;">11 </span> <span style="color: #0000ff;">'caller_get_posts'</span> <span style="color: #000000;">=&gt;</span> <span style="color: #0000ff;">1</span><span style="color: #000000;">,</span>
<span style="color: #008800; font-style: italic;">12 </span> <span style="color: #0000ff;">'orderby'</span> <span style="color: #000000;">=&gt;</span> <span style="color: #0000ff;">'rand'</span>);
<span style="color: #008800; font-style: italic;">13 </span> <span style="color: #000000;">query_posts</span>(<span style="color: #000000;">$args</span>);
<span style="color: #008800; font-style: italic;">14 </span> <span style="color: #000080; font-weight: bold;">if</span> (<span style="color: #000000;">have_posts</span>()) <span style="color: #000000;">:</span>
<span style="color: #f810b0;">15 </span> <span style="color: #000080; font-weight: bold;">while</span> (<span style="color: #000000;">have_posts</span>()) <span style="color: #000000;">:</span> <span style="color: #000000;">the_post</span>(); <span style="color: #000000;">update_post_caches</span>(<span style="color: #000000;">$posts</span>); <span style="color: #008080;">?&gt;</span>
<span style="color: #008800; font-style: italic;">16 </span> <span style="color: #000000;">&lt;li&gt; </span>
<span style="color: #008800; font-style: italic;">17 </span> <span style="color: #008080;">&lt;?php</span> <span style="color: #000000;">the_time</span>(<span style="color: #0000ff;">'Y/m/j'</span>); <span style="color: #008080;">?&gt;</span><span style="color: #000000;"> &lt;font class="pinkspec"&gt;--&lt;/font&gt; &lt;a href="</span><span style="color: #008080;">&lt;?php</span> <span style="color: #000000;">the_permalink</span>(); <span style="color: #008080;">?&gt;</span><span style="color: #000000;">" rel="bookmark" title="</span><span style="color: #008080;">&lt;?php</span> <span style="color: #000000;">the_title_attribute</span>();
<span style="color: #008800; font-style: italic;">18 </span> <span style="color: #008080;">?&gt;</span><span style="color: #000000;">"&gt;</span><span style="color: #008080;">&lt;?php</span> <span style="color: #000000;">the_title</span>(); <span style="color: #008080;">?&gt;</span><span style="color: #000000;">&lt;/a&gt;</span>
<span style="color: #008800; font-style: italic;">19 </span> <span style="color: #000000;">&lt;/li&gt;</span>
<span style="color: #f810b0;">20 </span> <span style="color: #008080;">&lt;?php</span> <span style="color: #000080; font-weight: bold;">endwhile</span>; <span style="color: #000080; font-weight: bold;">else</span> <span style="color: #000000;">:</span> <span style="color: #008080;">?&gt;</span>
<span style="color: #008800; font-style: italic;">21 </span> <span style="color: #000000;">&lt;li&gt;*暂无相关文章*&lt;/li&gt;</span>
<span style="color: #008800; font-style: italic;">22 </span> <span style="color: #008080;">&lt;?php</span> <span style="color: #000080; font-weight: bold;">endif</span>; <span style="color: #000000;">wp_reset_query</span>(); <span style="color: #000000;">}</span> <span style="color: #008080;">?&gt;</span>
<span style="color: #008800; font-style: italic;">23 </span> <span style="color: #000000;">&lt;/ul&gt;</span>
<span style="color: #008800; font-style: italic;">24 </span> <span style="color: #000000;">&lt;/div&gt;</span></div>
只需要注意$args = array(
'showposts' =&gt; 6,
'caller_get_posts' =&gt; 1,
'orderby' =&gt; 'rand');
query_posts($args);
这里，query_posts()可以决定哪些文章出现在循环中
参数和用法等距离，以下例子出自网络
类别 参数

显示属于某个类别的文章

cat
category_name
根据ID显示一个类别

只显示来自一个类别ID的文章

query_posts('cat=4');
根据名称显示一个类别

只显示属于某个类别名的文章

query_posts('category_name=Staff Home');
显示几个类别及ID

显示属于几个类别ID的文章

query_posts('cat=2,6,17,38');
删除某个类别的文章

显示所有的文章，但是类别ID前面有个’-’（负号）负号的类被除外。

query_posts('cat=-3');
删除属于类别3的所有文章。有一个限制性条款：会删除只属于类别3的所有文章。如果一个类别也同时属于其它的类别，这个类别仍然不会被删除。

标签参数

显示与某个标签相关的文章

tag
为某个标签提取文章

query_posts('tag=cooking');
获得拥有任何这样的标签的文章

query_posts('tag=bread,baking');
获取拥有这三个标签的文章

query_posts('tag=bread+baking+recipe');
作者参数

你也可以根据作者限制文章数目

author_name=Harriet
author=3
author_name在 user_nicename区操作, 同时作者 在作者id上操作。

文章 &amp; 网页参数

返回一篇单独的文章或者一个单独的网页

p=1 – 使用文章 ID来显示第一篇文章
name=first-post – 使用 post Slug 显示第一篇文章
page_id=7
pagename=about
showposts=1 (你可以使用 showposts=3,或者其它的任何数字显示一定数目的文章)
由于 模板层级方面的原因, home.php先执行了。这意味这你可以编写一个home.php，home.phh调用query_posts()重新得到一个特别的网页并且将那个网页设置为你的首页。没有任何插件或者hacks，你需要运行一个机制，并且显示和维护一个非博客的首页。

更有用的方法，可能是利用WP的网页功能并且为你的首页使用这个功能。你可以将”关于网页”设置为entry point或者设置为站点的末页。你可能执行一些更动态的步骤，设置一个自定义网页，显示最近的评论，文章，类别，存档。请看看下面的例子。

时间参数

得到某个特别的时间段内发表的文章

hour=
minute=
second=
day= – 一个月中的每一天; 显示，例如，十五号发表的所有文章。
monthnum=
year=
网页参数

paged=2 -显示使用”以前发表的文章”链接时，通常在网页2上显示的文章。
posts_per_page=10 -每个网页显示的文章数目；-1这个值，会显示所有的文章。
order=ASC -按时间顺序显示文章，以相反的顺序显示DESC（默认）
Offset 参数

你不能转移或者忽视一个或者更多的原始文章，这些文章一般是你的query同时使用offset参数收集到的。

下面的函数会显示（1）最近的5篇文章

query_posts('showposts=5&amp;offset=1');
根据参数排序

根据这个区给得到的文章排序

orderby=author
orderby=date
orderby=category
orderby=title
orderby=modified
orderby=modified
orderby=menu_order
orderby=parent
orderby=ID
orderby=rand
同时考虑”ASC”或者的”DESC”的排序参数

联合参数

你可能从上面的例子中注意到，你使用一个&amp;（&amp;符号）将参数组合在一起，像：

query_posts('cat=3&amp;year=2004');
类别13，关于当前月份显示在主页上的文章：

if (is_home())  {
query_posts ($query_string . '&amp;cat=13&amp;monthnum=' . date('n',current_time('timestamp'))); }
在2.3版本中，这个参数组合会返回属于类别1同时属于类别3的文章，只显示两篇（2）文章，根据标题，按降序排列：

query_posts(array('category__and'=&gt;array(1,3),'showposts'=&gt;2,'orderby'=&gt;title,'order'=&gt;DESC));
在2.3和2.5版本中，你可能期待下面的内容，返回属于类别1并且标签为”苹果”的所有文章

query_posts('cat=1&amp;tag=apples');
一个bug阻止这个运行。请看看Ticket #5433，一个工作区要搜索几个使用+的标签

query_posts('cat=1&amp;tag=apples+apples');
对于先前的查询，这个会产生期待的结果。注意使用’cat=1&amp;tag=apples+oranges’能够产生期待的结果。

-----------------------------------------------------------------------------------

所以，如果想输出随机文章，只要加orderby=rand就行了
文章输出之后，再简单地用css装饰下，相关文章和随机文章就做好了。