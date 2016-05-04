---
title: Android禁用返回键
tags:
  - android
  - 禁用返回键
id: 222
categories:
  - Android
date: 2011-07-08 09:59:45
---

开发应用的时候，有时需要禁用Android的返回键
比如全屏广告展示的时候防止用户按返回退出
实际上禁用返回键的方法有很多，这里列出一种
就是重写dispatchKeyEvent方法
判断如果按键事件是返回键
则返回true
就可以达到禁用返回键的目的
<pre name="code" class="java">
	@Override
	public boolean dispatchKeyEvent(KeyEvent event) {
		if (event.getKeyCode() == KeyEvent.KEYCODE_BACK) {
			return true;
		}
		return super.dispatchKeyEvent(event);
	}
</pre>