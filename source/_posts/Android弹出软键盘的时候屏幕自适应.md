---
title: Android弹出软键盘的时候屏幕自适应
tags:
  - android
  - 自适应
  - 软键盘
id: 188
categories:
  - Android
date: 2011-03-13 13:05:28
---

[![1](http://tinone.net/wp-content/uploads/2011/03/1_thumb.jpg "1")](http://tinone.net/wp-content/uploads/2011/03/1.jpg)[![2](http://tinone.net/wp-content/uploads/2011/03/2_thumb.jpg "2")](http://tinone.net/wp-content/uploads/2011/03/2.jpg)

最近在做东西的时候（一直都是）又被UI卡住   
要做到这种效果其实很容易（但是卡了我很久）    
难道以后还是应该优先搜索解决方案吗，而不是自己先试试....自己试效率真的很低    
只需要在Androidmanifest.xml定义Activity的时候增加    
android:windowSoftInputMode=&quot;stateVisible|adjustResize&quot;    
就能自适应    
如果不增加的话，软键盘会把最下面的toolBar覆盖
