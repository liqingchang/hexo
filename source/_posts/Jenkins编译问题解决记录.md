---
title: Jenkins编译问题解决记录
date: 2017-04-24 10:56:46
tags: android
---
公司的新项目在jenkins做CI的时候报了个这样的问题：
> * What went wrong:
A problem occurred configuring project ':TuringcatHbTV'.
> You have not accepted the license agreements of the following SDK components:
  [ConstraintLayout for Android 1.0.2, Solver for ConstraintLayout 1.0.2].
  Before building your project, you need to accept the license agreements and complete the installation of the missing components using the Android Studio SDK Manager.
  Alternatively, to learn how to transfer the license agreements from one workstation to another, go to http://d.android.com/r/studio-ui/export-licenses.html
  
解决方法如下：
1.执行
<pre>
echo y | android update sdk --no-ui --all --filter "tool,extra-android-m2repository,extra-android-support,extra-google-google_play_services,extra-google-m2repository"
</pre>

2.再执行
<pre>
echo y | $ANDROID_HOME/tools/bin/sdkmanager "extras;m2repository;com;android;support;constraint;constraint-layout-solver;1.0.2"
</pre>

然后做个Retrigger就没问题了