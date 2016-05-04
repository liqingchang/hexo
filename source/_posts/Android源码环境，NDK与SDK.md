---
title: Android源码环境，NDK与SDK
tags:
  - android
  - 环境搭建
id: 145
categories:
  - Code
date: 2011-02-27 09:37:59
---

最近在学习Android的开发，重点了解了Android编码环境的不同
首先看看Android源码环境，NDK与SDK分别有什么不同
SDK是我们在windows下编码的环境，大部分开发者都是用SDK进行开发的
NDK全称Native Development Kit，它的主要作用是允许开发者用原生代码（C/C++）进行开发，好处是更高的执行效率
源码环境跟NDK有相似的地方，但是实际上区别也很明显，我使用之后得出的最大区别是程序结构上的不同，另外android.mk文件的内容差别也十分大，总的来讲，大部分人都用不上源码环境，实际上SDK已经能满足大部分开发者的需要了

* * *

# 环境搭建

SDK环境搭建十分简单，基本不会出错，所以就不讲什么了，重点讲讲源码环境和NDK的搭建

##### 搭建NDK编码环境

首先安装Cygwin，它是一个在windows下运行unix的模拟环境，我们需要在Cygwin下进行makefile
我是在[Cygwin官网](http://www.cygwin.com/)进行下载的，下载完之后从网络安装，然后打了几盘dota就安装好了，实际上不需要完全安装，但是完全安装是最省事的，我很懒

[![image](http://tinone.net/wp-content/uploads/2011/02/image_thumb.png "image")](http://tinone.net/wp-content/uploads/2011/02/image.png)

安装完之后进入效果如图，我们先看看Cygwin是不是已经安装好了

[![image](http://tinone.net/wp-content/uploads/2011/02/image_thumb1.png "image")](http://tinone.net/wp-content/uploads/2011/02/image1.png)

<!--.csharpcode, .csharpcode pre { 	font-size: small; 	color: black; 	font-family: consolas, "Courier New", courier, monospace; 	background-color: #ffffff; 	/*white-space: pre;*/ } .csharpcode pre { margin: 0em; } .csharpcode .rem { color: #008000; } .csharpcode .kwrd { color: #0000ff; } .csharpcode .str { color: #006080; } .csharpcode .op { color: #0000c0; } .csharpcode .preproc { color: #cc6633; } .csharpcode .asp { background-color: #ffff00; } .csharpcode .html { color: #800000; } .csharpcode .attr { color: #ff0000; } .csharpcode .alt  { 	background-color: #f4f4f4; 	width: 100%; 	margin: 0em; } .csharpcode .lnum { color: #606060; } -->
<div id="codeSnippetWrapper" style="text-align: left; line-height: 12pt; background-color: #f4f4f4; margin: 20px 0px 10px; width: 97.5%; font-family: 'Courier New', courier, monospace; direction: ltr; max-height: 200px; font-size: 8pt; overflow: auto; cursor: text; border: silver 1px solid; padding: 4px;">
<div id="codeSnippet" style="text-align: left; line-height: 12pt; background-color: #f4f4f4; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;">
<pre style="text-align: left; line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;"><span id="lnum1" style="color: #606060;">   1:</span> cygcheck -c cygwin</pre>
<!--CRLF-->
<pre style="text-align: left; line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;"><span id="lnum2" style="color: #606060;">   2:</span> gcc --version</pre>
<!--CRLF-->
<pre style="text-align: left; line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;"><span id="lnum3" style="color: #606060;">   3:</span> make --version</pre>
<!--CRLF-->
<pre style="text-align: left; line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;"><span id="lnum4" style="color: #606060;">   4:</span> gdb --version</pre>
<!--CRLF-->

</div>
</div>
如图所示的话,表示cygwin已经安装完毕

下一步我们修改cygwin的安装目录下的home用户名.bash_profile文件

<span style="color: #ff0000;">如果没有找到这个文件，删除用户名文件夹，再重启cygwin就可以生成了

</span><span style="color: #000000;">打开后添加ndk=/cygdrive/盘符/ndk目录

export ndk</span>

<span style="color: #000000;">比如我的

ndk=/cygdrive/d/Java/Android/NDK/android-ndk-r5b

export ndk

</span>

<span style="color: #000000;">然后在cygwin输入cd $ndk</span><span style="color: #000000;">

</span>

[![image](http://tinone.net/wp-content/uploads/2011/02/image_thumb2.png "image")](http://tinone.net/wp-content/uploads/2011/02/image2.png)

如图所示表示设置成功了

之后可以进入sample目录之后执行$ndk/ndk-build就能进行编译了

[![image](http://tinone.net/wp-content/uploads/2011/02/image_thumb3.png "image")](http://tinone.net/wp-content/uploads/2011/02/image3.png)至此，NDK环境已经做好

* * *
再讲讲源码环境的搭建

源码环境能对Android系统本身进行编译

在搭建源码环境之前，我们需要用虚拟机虚拟出Ubuntu系统

这个我就不再唠叨了，我用的官方下载的virtualbox和官方的Ubuntu，我喜欢官方的东西

当然，源码环境也是参考官方的，不过这个官方教程不太好而已

搭建源码环境前，要了解这点

<span style="color: #ff0000;">我们需要哪个版本的Android，因为环境搭建时间很长，Android2.3编译的程序2.2是不能运行的，所以如果没有用到2.3的新特性，建议搭建2.2的环境

另外2.3的环境需要64位的Ubuntu和Jdk1.6来进行编译，当然，64位是可以改成32位的</span>

<span style="color: #ff0000;"> </span>

<span style="color: #ff0000;"> </span><span style="color: #ff0000;"><span style="color: #000000;">我们一步步走吧

首先获得root</span></span>

<span style="color: #ff0000;"><span style="color: #000000;">输入sudo passwd root

然后输入密码，之后注销用root登陆

然后要做的是安装需要的程序

</span><span style="color: #000000;"> </span>
<div id="codeSnippetWrapper" style="text-align: left; line-height: 12pt; background-color: #f4f4f4; margin: 20px 0px 10px; width: 97.5%; font-family: 'Courier New', courier, monospace; direction: ltr; max-height: 200px; font-size: 8pt; overflow: auto; cursor: text; border: silver 1px solid; padding: 4px;">
<div id="codeSnippet" style="text-align: left; line-height: 12pt; background-color: #f4f4f4; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;">
<pre style="text-align: left; line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;"><span id="lnum1" style="color: #606060;">   1:</span> sudo add-apt-repository <span style="color: #006080;">"deb http://archive.ubuntu.com/ubuntu dapper main multiverse"</span></pre>
<!--CRLF-->
<pre style="text-align: left; line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;"><span id="lnum2" style="color: #606060;">   2:</span> sudo add-apt-repository <span style="color: #006080;">"deb http://archive.ubuntu.com/ubuntu dapper-updates main multiverse"</span></pre>
<!--CRLF-->
<pre style="text-align: left; line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;"><span id="lnum3" style="color: #606060;">   3:</span> sudo apt-get update</pre>
<!--CRLF-->
<pre style="text-align: left; line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;"><span id="lnum4" style="color: #606060;">   4:</span> sudo apt-get install sun-java5-jdk</pre>
<!--CRLF-->
<pre style="text-align: left; line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;"><span id="lnum5" style="color: #606060;">   5:</span> sudo update-java-alternatives -s java-1.5.0-sun</pre>
<!--CRLF-->
<pre style="text-align: left; line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;"><span id="lnum6" style="color: #606060;">   6:</span> $ sudo apt-get install git-core gnupg flex bison gperf libsdl-dev libesd0-dev libwxgtk2.6-dev build-essential zip curl libncurses5-dev zlib1g-dev</pre>
<!--CRLF-->

</div>
</div>
<span style="color: #000000;">然后就可以开始下载Android的源码了

</span>
<div id="codeSnippetWrapper" style="text-align: left; line-height: 12pt; background-color: #f4f4f4; margin: 20px 0px 10px; width: 97.5%; font-family: 'Courier New', courier, monospace; direction: ltr; max-height: 200px; font-size: 8pt; overflow: auto; cursor: text; border: silver 1px solid; padding: 4px;">
<div id="codeSnippet" style="text-align: left; line-height: 12pt; background-color: #f4f4f4; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;">
<pre style="text-align: left; line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;"><span style="color: #000000;"><span id="lnum1" style="color: #606060;">   1:</span> cd ~</span></pre>
<!--CRLF-->
<pre style="text-align: left; line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;"><span style="color: #000000;"><span id="lnum2" style="color: #606060;">   2:</span> mkdir bin</span></pre>
<!--CRLF-->
<pre style="text-align: left; line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;"><span style="color: #000000;"><span id="lnum3" style="color: #606060;">   3:</span> PATH=~/bin/$PATH</span></pre>
<!--CRLF-->
<pre style="text-align: left; line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;"><span style="color: #000000;"><span id="lnum4" style="color: #606060;">   4:</span> curl http:<span style="color: #008000;">//android.git.kernel.org/repo &gt; ~/bin/repo</span></span></pre>
<!--CRLF-->
<pre style="text-align: left; line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;"><span style="color: #000000;"><span id="lnum5" style="color: #606060;">   5:</span> chmod a+x ~/bin/repo</span></pre>
<!--CRLF-->
<pre style="text-align: left; line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;"><span style="color: #000000;"><span id="lnum6" style="color: #606060;">   6:</span> mkdir android</span></pre>
<!--CRLF-->
<pre style="text-align: left; line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;"><span style="color: #000000;"><span id="lnum7" style="color: #606060;">   7:</span> cd android</span></pre>
<!--CRLF-->
<pre style="text-align: left; line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;"><span style="color: #000000;"><span id="lnum8" style="color: #606060;">   8:</span> repo init -u git:<span style="color: #008000;">//android.git.kernel.org/platform/manifest.git -b android-2.2_r1.3</span></span></pre>
<!--CRLF-->
<pre style="text-align: left; line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;"><span style="color: #000000;"><span id="lnum9" style="color: #606060;">   9:</span> repo sync</span></pre>
<!--CRLF-->

</div>
</div>
<span style="color: #000000;">然后开始漫长的等待吧，3M网速大概3~4小时就行了

中间如果出问题断了可以再repo sync继续的</span>

<span style="color: #000000;">下载完之后直接make就可以make出android系统了

实际上如果你只是想编译其中的某个程序或者其他目录的某个程序，可以参考下面的资料

输入. build/envsetup.sh之后，就可以用以下的命令进行编译
<div id="codeSnippetWrapper" style="text-align: left; line-height: 12pt; background-color: #f4f4f4; margin: 20px 0px 10px; width: 97.5%; font-family: 'Courier New', courier, monospace; direction: ltr; max-height: 200px; font-size: 8pt; overflow: auto; cursor: text; border: silver 1px solid; padding: 4px;">
<div id="codeSnippet" style="text-align: left; line-height: 12pt; background-color: #f4f4f4; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;">
<pre style="text-align: left; line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;"><span id="lnum1" style="color: #606060;">   1:</span> - croot:   Changes directory to the top of the tree.</pre>
<!--CRLF-->
<pre style="text-align: left; line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;"><span id="lnum2" style="color: #606060;">   2:</span> - m:       Makes from the top of the tree.</pre>
<!--CRLF-->
<pre style="text-align: left; line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;"><span id="lnum3" style="color: #606060;">   3:</span> - mm:      Builds all of the modules <span style="color: #0000ff;">in</span> the current directory.</pre>
<!--CRLF-->
<pre style="text-align: left; line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;"><span id="lnum4" style="color: #606060;">   4:</span> - mmm:     Builds all of the modules <span style="color: #0000ff;">in</span> the supplied directories.</pre>
<!--CRLF-->
<pre style="text-align: left; line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;"><span id="lnum5" style="color: #606060;">   5:</span> - cgrep:   Greps on all local C/C++ files.</pre>
<!--CRLF-->
<pre style="text-align: left; line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;"><span id="lnum6" style="color: #606060;">   6:</span> - jgrep:   Greps on all local Java files.</pre>
<!--CRLF-->
<pre style="text-align: left; line-height: 12pt; background-color: white; margin: 0em; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;"><span id="lnum7" style="color: #606060;">   7:</span> - resgrep: Greps on all local res/*.xml files.</pre>
<!--CRLF-->
<pre style="text-align: left; line-height: 12pt; background-color: #f4f4f4; margin: 0em; width: 100%; font-family: 'Courier New', courier, monospace; direction: ltr; color: black; font-size: 8pt; overflow: visible; border-style: none; padding: 0px;"><span id="lnum8" style="color: #606060;">   8:</span> - godir:   Go to the directory containing a file.</pre>
<!--CRLF-->

</div>
</div>
</span><span style="color: #000000;"> </span>

* * *
</span><span style="color: #000000;">做的时候觉得很多东西要写，写的时候发现其实也不多

源码环境其实并不复杂，多试试就好

实际上，我们用SDK已经足够了，对NDK和源码环境，先了解，等以后进阶的时候再好好研究吧

</span>

<span style="color: #000000;"> </span>