---
title: 'Wordpress中RSS,Feed优化报告(标题必须要长)'
tags:
  - Feed
  - htaccess
  - RSS
  - Wordpress
id: 107
categories:
  - Wordpress
date: 2011-01-09 01:06:25
---

最近获取信息的主要途径是GoogleReader，因此越来越觉得搞好自己的Blog的Feed十分重要。

### 域名绑定

Wordpress默认的rss地址是domain/feed，虽然在IE能解析，但是用Chrome就是一串数字，十分难看

于是需要借助feedsky或者feedburner来完善我们的RSS

申请和使用十分简单也很多教程，不一一讲了，这里用feedsky做例子

首先要在域名服务器添加一个子域名，比如我用feeds.tinone.net子域名作为我的永久rss地址

于是我在hostmonster增加了feeds.tinone.net的子域名，并将其CNAME到mydomain.feedsky.com

然后回到feedsky的控制面板如图

[![1](http://tinone.net/wp-content/uploads/2011/01/1_thumb.jpg "1")](http://tinone.net/wp-content/uploads/2011/01/1.jpg)

需要注意的是服务器应用CNAME是需要时间的，大家可以去看个电影什么的回来基本OK了，输入刚刚增加的子域名feeds.tinone.net，成功后feeds.tinone.net就可以自动转向到feedsky的feed地址了。

好处：我们这么折腾做这些动作，好处是以后feedsky如果抽风或者有什么意外，又或者你心血来潮不想用feedsky来托管，但是如果你换了托管商，feed地址改变了，原来订阅的用户就等于白白流失了，为了防止这种悲剧发生，于是我们绑定用户订阅的feed地址为自己的子域名，那么如果我们要换托管商，只要把子域名改绑为该域名商的feed地址，就既不会流失原来订阅的用户，又能达到目的了。

子域名就是一个变量，托管商就是变量的值，这样理解吧！

### 

### 模板修改

域名绑定之后，就可以开始修改模板了，要修改的地方是所有涉及到feed的页面，一般默认会用bloginfo('rss2_url')来获得wordpress的默认地址，比如

&lt;div class="sidebar-feeds"&gt;

&lt;a href="&lt;?php bloginfo('rss2_url')?&gt;" target="_blank" style="color:#F36E30;text-decoration:none;"&gt;订阅 RSS feed&lt;/a&gt;

&lt;/div&gt;

就是直接把链接链接到默认的wordpress的feed地址，我们只需要把"&lt;?php bloginfo('rss2_url')?&gt;" 改成刚刚增加的子域名"http://feeds.tinone.net"a就OK了，当然，模板所有用到这个函数的rss都需要修改。对之前帮一个朋友[Tina](http://tinali.net)改的模板CodeNameH相对比较简洁，所以这样hardcode没问题，但是我现在用的模板十分混乱，也不知道有多少地方用到了bloginfo('rss2_url')也不想一一修改，于是决定用其他方法。

### 

### .htaccess文件重定向

刚开始的想法是修改wordpress源程序的feed默认地址为想要的地址，但是那个太复杂了，果断放弃

google了下，发现可以在服务器配置.htaccess文件来达到重定向的目的

首先需要明白的是我们要用到的是[mod_rewrite模块](http://lamp.linux.gov.cn/Apache/ApacheMenu/mod/mod_rewrite.html)另外我们还需要一点[正则表达式的基本知识](http://hi.baidu.com/woshigao/blog/item/38364f50fc9e3e5a1138c209.html)，然后就可以开始着手配置了。

先来个简单的

&lt;IfModule mod_rewrite.c&gt;
RewriteEngine On
RewriteBase /
RewriteRule .* [http://feeds.tinone.net](http://feeds.tinone.net) [L,R=307]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]
&lt;/IfModule&gt;

如果有做wordpress固定连接的话，会看到456行，无视它，我们只看第4行

利用RewriteRule我们将将匹配任意字符(.*)，如果url有任意字符，我们都转到[http://feeds.tinone.net](http://feeds.tinone.net)，好了，保存试试再去浏览你的主页，跳转到[http://feeds.tinone.net](http://feeds.tinone.net)了吧。

然后，我们要为RewriteRule增加一些条件，来控制当用户浏览feed页面的时候，跳转到[http://feeds.tinone.net](http://feeds.tinone.net)

做法很简单在，RewriteBase后面增加

RewriteCond %{REQUEST_URI} feed$ [NC]

REQUEST_URI会获得我们请求的字符串，然后回自动匹配如果最后是feed，那么就跳入下面的RewriteRule，否则不执行下面的RewriteRule。

再试试，现在我们成功进入我们原来的主页了，我们再单击RSS订阅，系统自动转入[http://feeds.tinone.net](http://feeds.tinone.net)表示我们已经成功重定向。

这样的好处是以后更改主题等都不用再改模板这么麻烦了，而且自由度十分高，可以尽情控制转向条件（利用RewriteCond）

其实RewriteRule feed$ [http://feeds.tinone.net](http://feeds.tinone.net) [L,R=307]也能达到目的，不过因为之前测试的时候出现了很多问题，导致RewriteCond不执行匹配或者匹配出错等，心里面强迫症一定要用RewriteCond来搞……