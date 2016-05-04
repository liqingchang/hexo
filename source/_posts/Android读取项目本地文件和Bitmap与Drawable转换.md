---
title: Android读取项目本地文件和Bitmap与Drawable转换
tags:
  - android
  - Bitmap
  - drawable
id: 215
categories:
  - Android
date: 2011-07-01 15:16:01
---

在做Android的Jar包的时候，有时候需要读取项目包里的图片文件（不能用到android生成的R文件，否则jar包无法使用）

方法也很简单，直接上代码

<pre name="code" class="java">
public Bitmap getPic(){
    Bitmap bitmap = BitmapFactory.decodeStream(getClass()
        .getResourceAsStream("pic.png"));
    return bitmap;
}
</pre>

另外，很多时候我们都需要在Drawable和Bitmap间转换

方法也很简单，Android提供了一个叫BitmapDrawable的类提供了响应功能

还是直接上代码
<pre name="code" class="java">
public Drawable bitmapToDrawable(Bitmap bitmap){
    Drawable drawable = new BitmapDrawable(bitmap);
    return drawable;
}

public Bitmap drawableToBitmap(Drawable drawable){
    bitmap = ((BitmapDrawable)drawable).getBitmap();
    return bitmap;
}
</pre>