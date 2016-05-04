---
title: Android单位区别与转化
tags:
  - android
  - dp
  - px
  - sp
  - 单位
id: 262
categories:
  - Android
date: 2011-08-02 18:44:45
---

Android设置有很多长度单位，dp、px等
px   像素，指屏幕上的一个点
in   英寸
mm   毫米
pt   磅，也就是1/72英寸
dp   有点类似于像素但是它这个像素和密度没有关系，是一种抽象单位，在每英寸160像素的屏幕上1dp=1px，如果在320像素的屏幕上，1dp=2px，也就是讲dp可以自适应大小
dip  这个和dp一样
sp   这个也和dp差不多但是它是和刻度无关，一般字体大小多用这个

这里选出最常用的px和dp，给出他们转化的方法
<pre name="code" class="java">
/**
* 根据手机的分辨率从 dp 的单位 转成为 px(像素)
*/
public static int dip2px(Context context, float dpValue) {
final float scale = context.getResources().getDisplayMetrics().density;
return (int) (dpValue * scale + 0.5f);
}

/**
* 根据手机的分辨率从 px(像素) 的单位 转成为 dp
*/
public static int px2dip(Context context, float pxValue) {
final float scale = context.getResources().getDisplayMetrics().density;
return (int) (pxValue / scale + 0.5f);
}
</pre>

要注意不要搞混哦！
