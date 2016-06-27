---
title: InstantRun和MultiDex调研
date: 2016-06-26 13:49:13
tags:
---

[官方说明戳我](https://developer.android.com/studio/build/multidex.html)
**前言**： 我司的项目由于各种各样的包引用，方法数早超64k，导致只能用Multidex支持多Dex编译防止崩溃。最近由于apk包大小暴涨，而公司使用的又是通过wifi连接的特殊硬件设备，因此当apk很大的时候，修改代码-编译-安装验证效果的成本也越来越大。周末没事回公司看看能不能解决这个问题，提高大家的效率，因此有这篇关于MultiDex的小文章。

---
#### 64k方法限制
假如你在编译你的app的时候，遇到了这样的错误
<pre>Conversion to Dalvik format failed:
Unable to execute dex: method ID not in [0, 0xffff]: 65536</pre>

又或者是这样的错误
<pre>trouble writing output:
Too many field references: 131000; max is 65536.
You may try using --multi-dex option.</pre>

出现这个错误的原因是，每个dex文件支持的方法数上限是64k
因此当你遇到这个错误，恭喜你，你写了很多（冗余）的代码，或者你引用了好多（不那么好的）库，也许也真的是你的项目很大很大....

在5.0(API level 21)版本之前，因为使用Dalvik runtime来运行app代码，因此默认每个apk限制只能有一个class.dex文件。为了解决这个问题，我们需要使用[multidex support library](https://developer.android.com/topic/libraries/support-library/features.html#multidex)，这里不详细叙述怎么去支持multidex，实际就是**简单地在build.gradle个multidex的enable，然后再在Application上做一个MultiDex的支持就行**。
而在5.0(Api level 21及21以上)之后，使用了新的ART(Android RunTime)黑科技来原生支持multidex。简单来说，ART会在安装的时候执行一次pre-compilation来扫描apk里的class(N).dex文件并把他们合成一个.oat文件。[关于ART更加详细的描述戳我](https://source.android.com/devices/tech/dalvik/index.html)

#### Multidex的缺点
很多同学会说，如果支持(解决)multidex这么简单，感觉我们也没有太大的损失啊，就支持以下咯。
但是实际上，强行支持Multidex会带来一些隐患，比如

* 如果第二个dex太大的话，安装时可能会导致ANR
* 对4.0(API level 14)以前的设备支持有问题
* multidex会请求大量内存达到Dalvik的线性内存请求达到限制导致崩溃（至于什么是线性内存请求限制，有兴趣的同学可以进一步查查，然后告诉我）
* 在Dalvik runtime使用主dex文件运作的时候，会有一个复杂的请求，我们不需要关注这个复杂的请求具体机制是什么，只需要知道这些在主dex的复杂请求可能会由于各种对Java方法的(自然)引用导致无法正常运作。(老实说这段我没怎么看懂....)
* 编译速度也会变慢

各位同学可能又会说，看上去也不是什么大问题啊？我们用得好好的。
您是认真的吗？？？

官网上的这段描述其实想表达的是，你可以这样用，这是我们的一个支持机制，但是它就像一个不知道什么时候会爆炸的炸弹，这个隐患可能会导致你的app瞬间被炸，也有可能不会。也就是说，如果你能接受未来可能出现的app的大坑，那么可以放心使用multidex，否则就应该好好做一下代码重构和编译优化。

#### 避免64k限制的出现
虽然64k限制可以用multidex的支持来解决，但是上面也说了，它也可能是个隐患。因此在可能的情况下，避免64k限制出现才是根本解决问题的方法。

##### 代码重构
很多优秀的框架都会用更多的类或方法的增加来遵循开闭原则，权衡以下，的确值得，而且我相信这不会是64k限制出现的原因。看看哪些模块生成了巨额方法引用，然后对这些模块做简单重构，可以带来不错的效果。但是老实说，国内的公司对这种非功能性的需求都不怎么重视，有这个重构时间，还不如实现多几个功能多捞点钱，对吧。

##### 编译优化
1. 检查项目的直接或间接引用
2. 使用ProGuard来移除不使用的代码和资源

配合我司项目的具体情况，ProGruad似乎会是最优解，但是我们还是要先看看关于InstantRun以及Multidex编译的一些优化内容

#### [Instant Run](https://developer.android.com/studio/run/index.html#instant-run)
简单来说就是可以让你不用每次都重新编译所有内容，大大大大地减少编译时间，具体的实现方法可以点击链接查看官网介绍，但是注意:
**Instant Run只支持debug版本，并且Isntant Run会直接把你的minSdkVersion提升为21，因此在release版本中还是需要做multidex的支持**

#### 优化MultiDex编译
在模块build.gradle文件中，添加对productFlavors参数，在debug版本，使用minSdk21，可以降低调试时候的编译时间，但是在release版本，换回我们的targetVersion就好。

<pre>
android {
    productFlavors {
        // Define separate dev and prod product flavors.
        dev {
            // dev utilizes minSDKVersion = 21 to allow the Android gradle plugin
            // to pre-dex each module and produce an APK that can be tested on
            // Android Lollipop without time consuming dex merging processes.
            minSdkVersion 21
        }
        prod {
            // The actual minSdkVersion for the application.
            minSdkVersion 14
        }
    }
          ...
    buildTypes {
        release {
            runProguard true
            proguardFiles getDefaultProguardFile('proguard-android.txt'),
                                                 'proguard-rules.pro'
        }
    }
}
dependencies {
  compile 'com.android.support:multidex:1.0.0'
}
</pre>

#### [ProGuard](https://developer.android.com/studio/build/shrink-code.html)
实际上，apk太大导致使用wifi传输apk时间过长，可能才是调试程序的最大问题。因此，怎么应用ProGuard来降低apk包大小，可能会比使用更新版本的编译更能提升调试效率。




