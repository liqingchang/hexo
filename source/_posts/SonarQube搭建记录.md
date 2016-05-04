---
title: SonarQube搭建记录
id: 423
categories:
  - Code
date: 2015-10-26 13:11:55
tags:
---

前言：之前还有jenkins和gerrit的搭建，但是太懒没记录....以后一定补上
SonarQube搭建记录

环境:ubuntu 14.04 LTS

1\. sonar本身安装十分简单，在http://www.sonarqube.org下载合适版本的文件，直接解压后运行bin/linux-x86-64/sonar.sh start，然后直接就可以在浏览器用localhost:9000看到界面，默认账户名密码是admin

2\. 然后安装sonar-runner，直接配置环境变量，在需要扫描的项目下创建对应的sonar-peoject.properties文件，执行sonar-runner，就能再sonar界面看到扫描结果。

问题：
在我的Android项目下用sonar-runner以Android Lint的sonar.profile执行扫描，扫描不出issue，实际上直接用gradle :lint是可以扫出一堆问题的
再google、stackoverflow找了好久都没有找到解决方法
先改用gradle来执行sonar看看，执行方法如下：

根据http://docs.sonarqube.org/display/PLUG/Android+Lint+Plugin的描述
需要用Gradle SonarQube plugin来扫描项目
http://docs.sonarqube.org/display/SONAR/Analyzing+with+Gradle
有具体步骤
1\. 按照https://plugins.gradle.org/plugin/org.sonarqube配置build.gradle，如果是2.1版本以上的gradle，直接在build.gradle添加
plugins {
  id "org.sonarqube" version "1.0"
}

可以用gradle -v查看gradle版本，这里用的是gradle2.4

2\. 配置完后直接执行gradle sonarqube，果断报错，提示
only buildscript {} and other plugins {} script blocks are allowed before plugins {} blocks, no other statements are allowed
修改一下plugins{}这个blocks的位置到文件最前面解决这个错误

3\. 运行成功后就可以在localhost:9000看到项目描述，但是现在还没有文件被扫描，因为没有指定嘛，跟着教程走，添加相应的路径
sonarqube {
    properties {
        property "sonar.sourceEncoding", "UTF-8"
        property "sonar.sources", "src"
        property "sonar.profile", "Android Lint"
        property "sonar.android.lint.report","build/outputs/lint-results.xml"
    }
}
后3行就是指：源码路径、扫描规则指定以及lint输出文件路径

4\. 再执行一次gradle sonarqube，成功

5\. 由于我的项目是多module项目，刚刚是在一个子模块subModule进行扫描成功，当我在根目录进行相同配置和扫描的时候，报了一个新从错误：
all buildscript {} blocks must appear before any plugins {} blocks in the script
看来根plugins这个blocks不能写在根目录的buildscript之前

6\. 修改一下再运行一遍gradle sonarqube，报了刚刚subModule1的中build.gradle的错误
Plugin 'org.sonarqube' is already on the script classpath. Plugins on the script classpath cannot be applied in the plugins {} block. Add  "apply plugin: 'org.sonarqube'" to the body of the script to use the plugin.
看来根目录声明过sonarqube的plugin后子module的build.gradle不需要再声明了
修改为apply plugin:'org.sonarqube'，再执行gradle sonarqube

7\. Cannot add extension with name 'sonarqube', as there is an extension already registered with that name
这个一下子不知道什么意思，先返回subModule再做一次gradle sonarqube看看

8\. > Failed to apply plugin [id 'org.sonarqube']
   > Cannot add extension with name 'sonarqube', as there is an extension already registered with that name.
嗯，有点乱，用--stacktrace和--info查看问题原因
9\. 原来是刚刚subModule已经创建过subModule的报告，所以报错，在sonar删除刚刚子module的报告，再执行一次gradle sonarqube，终于success

10\. 但是网页一看，居然没扫到文件，这是个问题，还是--stacktrace和--info看看，原来sonar.profile的Android Lint出问题，在网页把默认profile改为Android Lint，然后去掉这个tag，扫描成功，有文件，但是居然扫到的是0个issue，这不可能

11\. 中间各种问题没有一一记录，但是初步认为是项目结构不良导致的

30分钟后，把项目结构修改了

12\. lint编译，sonar编译，有了，但是还是很少应该没问题了

13\. 以Android Lint做sonar扫描，一定要先做gradle lint，生成Lint文档，否则找不到问题，问题关键就是这里