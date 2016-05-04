---
title: Parcelable接口应用
tags:
  - android
  - Bitmap
  - Parcelable
id: 207
categories:
  - Android
date: 2011-04-21 11:58:21
---

Android中Intent传递对象有两个方法，一个是让对象实现Serializable接口，另一个是让对象实现Parcelable接口，Sample的话Google搜一下就很多了。   
大部分情况，Sample都是传递一个只有StringInt等基本类型的对象，如果需要传递图片的话，比如要传递Bitmap，用Parcelable接口的话，会比较容易，也不用特意转成数据流（其实是我不知道怎么用Serizlizable实现）。

似乎直接上代码比较好

import android.graphics.Bitmap;   
import android.os.Parcel;    
import android.os.Parcelable; 

public class ParceBean implements Parcelable{   
&#160;&#160;&#160; private&#160; Bitmap dw;    
&#160;&#160;&#160; private String name;    
&#160;&#160;&#160; public String getName() {    
&#160;&#160;&#160;&#160;&#160;&#160;&#160; return name;    
&#160;&#160;&#160; } 

&#160;&#160;&#160; public void setName(String name) {   
&#160;&#160;&#160;&#160;&#160;&#160;&#160; this.name = name;    
&#160;&#160;&#160; } 

&#160;&#160;&#160; public Bitmap getDw() {   
&#160;&#160;&#160;&#160;&#160;&#160;&#160; return dw;    
&#160;&#160;&#160; } 

&#160;&#160;&#160; public void setDw(Bitmap dw) {   
&#160;&#160;&#160;&#160;&#160;&#160;&#160; this.dw = dw;    
&#160;&#160;&#160; } 

&#160;&#160;&#160;&#160; public static final Parcelable.Creator&lt;ParceBean&gt; CREATOR = new Creator&lt;ParceBean&gt;() {&#160; 
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; public ParceBean createFromParcel(Parcel source) {&#160; 
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; ParceBean pb = new ParceBean();&#160; 
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; pb.name = source.readString();&#160; 
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; pb.dw = Bitmap.CREATOR.createFromParcel(source);    
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; return pb;&#160; 
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; }&#160; 
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; public ParceBean[] newArray(int size) {&#160; 
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; return new ParceBean[size];&#160; 
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; }&#160; 
&#160;&#160;&#160;&#160;&#160;&#160;&#160; };&#160; 
&#160;&#160;&#160; @Override    
&#160;&#160;&#160; public int describeContents() {    
&#160;&#160;&#160;&#160;&#160;&#160;&#160; return 0;    
&#160;&#160;&#160; } 

&#160;&#160;&#160; @Override   
&#160;&#160;&#160; public void writeToParcel(Parcel parcel, int flags) {    
&#160;&#160;&#160;&#160;&#160;&#160;&#160; parcel.writeString(name);    
&#160;&#160;&#160;&#160;&#160;&#160;&#160; dw.writeToParcel(parcel, 0);    
&#160;&#160;&#160; }    
}

Bitmap本身实现了Parcelable接口，利用writeToParccel之后可以用createFromParcel来rebuild这个Bitmap。   
然后再Activity调用这个JavaBean就可以了。