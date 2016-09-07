---
title: 关于complieSdkVersion、targetSdkVersion和minSdkVersion
tags: android
---
[参考源文](https://medium.com/google-developers/picking-your-compilesdkversion-minsdkversion-targetsdkversion-a098a0341ebd#.tz5zzucma)

在用gradle做Android编译的时候一定会遇到compileSdkVersion, minSdkVersion和targetSdkVersion这几个参数
比如类似这样的build.gradle配置
<pre>
defaultConfig {
        minSdkVersion 19
        targetSdkVersion 21
    }
compileSdkVersion 21
buildToolsVersion "21.1.1"
</pre>
那么这几个参数应该怎么选择呢，参考源文作出了很详尽的解析
#### compileSdkVersion
**compileSdkVersion**用于告诉gradle用哪个版本的Android SDK来编译应用。如果想用新的Android SDK提供的API，就必须使用对应版本以上的compileSdkVersion。需要强调的是**改变compileSdkVersion不会改变运行时的环境**，虽然更新compileSdkVersion可能会导致新的警告/错误，但是实际编译的时候compileSdkVersion是不会包含在apk中的:它只用在编译的时候。也就是说，更新compileSdkVersion不会导致你的apk变得不可用或者出现什么新问题，但是新出现的警告是有原因的，应该注意并找机会修复。

无论什么情况下都建议使用最新的compileSdkVersion来作为编译环境进行编译，因为这样做可以得到最新的代码检测避免过期的的API以及随时可以使用新API。

需要注意的是，如果项目中使用了[Support Library](https://developer.android.com/topic/libraries/support-library/index.html)，使用最新的SDK版本作为compileSdkVersion属性进行编译的话，使用的Support Library也需要更新到最新的发布版本。

#### minSdkVersion
minSdkVersion表示的是**能运行应用的最低Android版本**。这个参数也会用于Google Play判断用户的设备能装哪些应用。
这个参数也是开发过程的一个重要参数：在做lint检测的时候，lint会根据minSdkVersion的值来警告你使用了高于minSdkVersion提供的APIs，这样可以避免运行过程中调用一个低版本不存在的API导致的错误。另外还要注意的一点是，[Support Library](https://developer.android.com/topic/libraries/support-library/index.html)和[Google Play services](https://developers.google.com/android/guides/overview)等都由它们自己的minSdkVersion，使用它们的时候，设定的minSdkVersion一定要比它们的minSdkVerion高。
在权衡新特性（新的SDK提供更多特性/API以及更好的稳定性）以及支持用户数的时候，可以参考谷歌提供的当前[Android版本分布页面](https://developer.android.com/about/dashboards/index.html)，这个页面提供最近7天Google Play统计的Android设备的版本分布。

#### targetSdkVersion
**targetSdkVersion是Android进行向前兼容（早期版本编译的程序能在新推出的环境中运行，比如用SDK版本为10开发的应用，能在安卓版本为19的设备上使用）的主要手段**，除非targetVersionSdk进行更新，否则即使使用新的complieSdkVersion进行编译，新的行为变更不会被应用。各个版本的[行为变更](https://developer.android.com/reference/android/os/Build.VERSION_CODES.html)有官方文档进行记录，一些重要的变更也会记录在[发布记录](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html)中页面。

比如，在[Android M的行为变更](https://developer.android.com/reference/android/os/Build.VERSION_CODES.html#M)中有表明，如果提供的TimeZone的值不属于Olson数据库，AlarmManager.setTimeZone()会失败。但是如果targetSDKVersion不是23，这个行为的改变不会生效。**更新targetSdkVersion一定要对应用做全面的测试 更新targetSdkVersion一定要对应用做全面的测试 更新targetSdkVersion一定要对应用做全面的测试 ****

#### 总结
正常情况下，你的build.gradle配置中，这3个值大概是这个样子
` minSdkVersion <= targetSdkVersion <= compileSdkVersion `
但是更好的方案是这样的
`minSdkVersion (lowest possible) <=  targetSdkVersion == compileSdkVersion (latest SDK)`
**使用低版本的minSdkVersion来覆盖更多的用户并提高targetSdkVersion和compileSdkVersion来获得更好的应用特性！**
