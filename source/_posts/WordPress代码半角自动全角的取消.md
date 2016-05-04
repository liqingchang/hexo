---
title: WordPress代码半角自动全角的取消
tags:
  - Wordpress
  - 代码
  - 全角
  - 半角
id: 289
categories:
  - Code
date: 2012-02-01 11:07:55
---

WordPress会自动把标点的半角转换为全角
部分读者复制代码之后还要手动把全角转回半角，这超蛋疼的
解决方法也很简单
修改 wp-includes/formatting.php 文件，把实现自动替换的相关语句注释掉
<pre name="code" class="java">
// static strings
$curl = str_replace($static_characters, $static_replacements, $curl);
// regular expressions
$curl = preg_replace($dynamic_characters, $dynamic_replacements, $curl);
</pre>
以上便是自动转换的代码，把2、4行也注释掉就行了
每次更新Wordpress之后都要做一次哦亲