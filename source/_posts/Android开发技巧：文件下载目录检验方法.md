---
title: Android开发技巧：文件下载目录检验方法
tags:
  - android
  - 开发
  - 技巧
id: 349
categories:
  - Android
  - Code
date: 2012-02-22 10:54:32
---

在做下载线程的时候，当然需要有一个方法来确保下载路径的存在

这个时候可以用这个方法

<pre lang="java" line="1">

//静态全局常量，表示下载文件夹

private final static String STORAGE_PATH = "download";

/**
* 创建下载路径对象，如果文件夹不存在，则创建文件夹，如果创建失败，就用sdcard根目录代替
* @return
*/
private File ensureDownloadsFolder(){
File downloads = new File(Environment.getExternalStorageDirectory(), STORAGE_PATH);
if(!downloads.exists()){
boolean succ = downloads.mkdir();
if(!succ){
//Cannot create, should not happen normally, return original directory
return Environment.getExternalStorageDirectory();
}
}
return downloads;
}

</pre>
