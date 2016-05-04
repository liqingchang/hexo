---
title: WebViewClient与WebChromeClient的区别
tags:
  - android
  - WebChromeClient
  - WebView
  - WebViewClient
id: 248
categories:
  - Android
date: 2011-07-26 14:21:11
---

Android应用开发的时候可能会用到WebView这个组件，使用过程中可能会接触到WebViewClient与WebChromeClient，那么这两个类到底有什么不同呢？

WebViewClient主要帮助WebView处理各种通知、请求事件的，比如：
<table border="1" cellspacing="0" width="640" cellpadding="0">
<tbody>
<tr>
<td valign="top">onLoadResource</td>
</tr>
<tr>
<td valign="top">onPageStart</td>
</tr>
<tr>
<td valign="top">onPageFinish</td>
</tr>
<tr>
<td valign="top">onReceiveError</td>
</tr>
<tr>
<td valign="top">onReceivedHttpAuthRequest</td>
</tr>
</tbody>
</table>
WebChromeClient主要辅助WebView处理Javascript的对话框、网站图标、网站title、加载进度等比如
<table border="1" cellspacing="0" width="640" cellpadding="0">
<tbody>
<tr>
<td valign="top">onCloseWindow(关闭WebView)</td>
</tr>
<tr>
<td valign="top">onCreateWindow()</td>
</tr>
<tr>
<td valign="top">onJsAlert (WebView上alert无效，需要定制WebChromeClient处理弹出)</td>
</tr>
<tr>
<td valign="top">onJsPrompt</td>
</tr>
<tr>
<td valign="top">onJsConfirm</td>
</tr>
<tr>
<td valign="top">onProgressChanged</td>
</tr>
<tr>
<td valign="top">onReceivedIcon</td>
</tr>
<tr>
<td valign="top">onReceivedTitle</td>
</tr>
</tbody>
</table>
看上去他们有很多不同，实际使用的话，如果你的WebView只是用来处理一些html的页面内容，只用WebViewClient就行了，如果需要更丰富的处理效果，比如JS、进度条等，就要用到WebChromeClient。
更多的时候，你可以这样
<pre name="code" class="java">WebView webView;
webView= (WebView) findViewById(R.id.webview); 
webView.setWebChromeClient(new WebChromeClient()); 
webView.setWebViewClient(new WebViewClient()); 
webView.getSettings().setJavaScriptEnabled(true); 
webView.loadUrl(url);</pre>
这样你的WebView理论上就能有大部分需要实现的特色了
当然，有些更精彩的内容还是需要你自己添加的