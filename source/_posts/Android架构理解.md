---
title: Android架构理解
tags:
  - android
  - 基础
  - 架构
id: 205
categories:
  - Android
date: 2011-03-28 21:13:23
---

在SDK的What’s Android?就首先谈到了Android的架构，这也是我今天笔试的一道题，我当时的答案在最后，而本文全部都是这道题的答案！

[![clip_image002](http://tinone.net/wp-content/uploads/2011/03/clip_image002_thumb.jpg "clip_image002")](http://tinone.net/wp-content/uploads/2011/03/clip_image002.jpg)

这幅图显示了Android的4层架构

从图中可以看出，Android架构一共分为四层

最底层的LinuxKernel，Linux字眼表明了Android的根本

Android是基于Linux2.6提供核心服务的，比如安全，内存管理，进程管理，网络堆栈和驱动模型（Android relies on Linux version 2.6 for core system services such as security, memory management, process management, network stack, and driver model. The kernel also acts as an abstraction layer between the hardware and the rest of the software stack.）

个人感觉最底层的开发跟源码环境息息相关，因为如果想更改Android核心的东西，就可能需要更改Android本身的源码，如果要更改源码，就必须在Linux下搭建Android的源码环境。

现在的我还没有到要接触这一层的高度。

第三层提供了Android系统各种组件使用的用C/C++实现的核心类库。比如C，媒体库，接口管理器，SGL（Android2D引擎），OpenGL ES还有SQLite等。

第三层还包括AndroidRuntime

每一个Android应用程序都有一个属于自己的进程，在属于他们自己的Dalvik虚拟机的实例中，Dalvik被设计成可以有效率地运行多个虚拟机。Dalvik虚拟机执行一种基于少量内存优化的.dex格式文件。

实际上大部分开发都不会触及第三层

个人认为第三层的开发跟NDK，Native Development Kit有关，通过NDK开发人员能够让Android开发原生支持C/C++。

第二层是ApplicationFramework，提供了一个开放的开发平台（也就是我们常用的SDK），Android提供给开发人员丰富的创新的优秀的应用，开发人员可以利用这些实现很多让人兴奋的功能！

实际上所有的应用都是一系列的服务和系统，包括

·一个丰富的可扩充的视图（Views），视图构建了系统的ＵＩ部分

·Content Provider，是应用程序之间访问和共享数据的桥梁

·资源管理器，访问非代码资源，比如我们定义的字符串表示，图形和布局等

·通知管理器，允许所有应用在状态栏显示通知

·Activity管理器，管理应用的生命周期和提供一个通用的navigation backstack（导航回退）功能等。这些可以先记住，或者先不记住，做过简单的普通的HelloWorld开发等再来看看这个，会慢慢变得清晰！

ApplicationsFramework这一层就是我们的SDK！

最上层是Applicatons，就是Android本来就提供的一些email客户端，短信程序，日历，地图，浏览器，通信录等东西，所有的这些Applications都是用Java语言编写的，可以在源码环境下看到他们的源码，这些源码也是我们Android开发的很好的范例！

* * *
下面是我今天对Android架构的回答，当我我根本不理解Android的架构，现在想想自己当时答的真的很胡闹....不过不懂就是不懂，建议大家学习是先要清晰得了解Android的架构，总得来讲，80%以上我们开发者是活在第二层的，另外我还需要对Android Dalvik虚拟机的运作，Android的线程等有更深入的了解。

当时答的大概是“Android架构分为4层，分别是Application,ApplicationFramework,xxx和Linux，其中第一二层是SDK能执行的，第三层是NDK所涉及的，第四层是源码环境所涉及的。”
