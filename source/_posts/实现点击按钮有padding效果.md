---
title: 实现点击按钮有padding效果
date: 2017-04-13 12:12:49
tags: android
---

在StackOverflow看到个[问题](http://stackoverflow.com/questions/43192378/how-to-properly-remove-padding-or-margin-around-buttons-in-android/43383320#43383320)主要就是希望点击有padding效果，简单实现了一下。

```
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:state_pressed="true">
             <shape>
				<solid android:color="#ff0000"/>
				<stroke android:width="5dp" android:color="#00ff00"/>
		</shape>
	</item>
	<item>
		<shape>
			<solid android:color="#00ff00"/>
		</shape>
	</item>
</selector>
```

直接使用这个background就好

<img src="/images/ButtonEffect/1.png" width="250"> <img src="/images/ButtonEffect/2.png" width="250">

github:https://github.com/liqingchang/ButtonEffect
