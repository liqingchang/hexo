---
title: 利用纵向Seekbar实现的音量控制器
tags:
  - android
  - SeekBar
  - VerticalSeekBar
  - 纵向进度条
id: 389
categories:
  - Android
  - Code
date: 2015-02-26 17:55:56
---

某平板应用的首页需要做一个音量控制器，ue大人给了一个这样的图

[![音量_01](http://www.4gun.net/wp-content/uploads/2015/02/音量_01.png)](http://www.4gun.net/wp-content/uploads/2015/02/音量_01.png)

&nbsp;

&nbsp;

&nbsp;

&nbsp;

1\. 纵向SeekBar实现

#### 2\. xml实现SeekBar的进度条样式和thumb

### 纵向SeekBar实现

<pre class="lang:java decode:true ">package com.terry.verticalbardemo.widget;

import android.content.Context;
import android.graphics.Canvas;
import android.util.AttributeSet;
import android.view.MotionEvent;
import android.widget.SeekBar;

/**
 * 纵向Seekbar
 * 参考自http://stackoverflow.com/questions/4892179/how-can-i-get-a-working-vertical-seekbar-in-android/7341546#7341546
 * Created by terry on 15-2-11.
 */
public class VerticalSeekBar extends SeekBar {

    public VerticalSeekBar(Context context) {
        super(context);
    }

    public VerticalSeekBar(Context context, AttributeSet attrs, int defStyle) {
        super(context, attrs, defStyle);
    }

    public VerticalSeekBar(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    protected void onSizeChanged(int w, int h, int oldw, int oldh) {
        super.onSizeChanged(h, w, oldh, oldw);
    }

    /**
     * 用于刷新thumb的位置
     */
    public void onSizeChanged() {
        onSizeChanged(getWidth(), getHeight(), 0, 0);
    }

    @Override
    protected synchronized void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(heightMeasureSpec, widthMeasureSpec);
        setMeasuredDimension(getMeasuredHeight(), getMeasuredWidth());
    }

    protected void onDraw(Canvas c) {
        c.rotate(-90);
        c.translate(-getHeight(), 0);
        super.onDraw(c);
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        if (!isEnabled()) {
            return false;
        }

        switch (event.getAction()) {
            case MotionEvent.ACTION_DOWN:
            case MotionEvent.ACTION_MOVE:
            case MotionEvent.ACTION_UP:
                setProgress(getMax() - (int) (getMax() * event.getY() / getHeight()));
                onSizeChanged(getWidth(), getHeight(), 0, 0);
                break;

            case MotionEvent.ACTION_CANCEL:
                break;
        }
        return true;
    }

}
</pre>
参考自stackoverflow中关于安卓VerticalSeekBar的[优秀回答](http://stackoverflow.com/questions/4892179/how-can-i-get-a-working-vertical-seekbar-in-android/7341546#7341546)
这个VerticalSeekBar巧妙地修改了SeekBar本身的横竖来达到把横向SeekBar变成纵向的效果，其中thumb改变的方法由于源码没有公开（见源码的AbsSeekBar.java）所以用onSizeChanged()代为触发。
根据需要添加了一个公开方法onSizeChanged()用于外部touch事件更新thumb的position。

### xml实现SeekBar的进度条样式和thumb

为了更好的应对频繁改动的需求，减轻ue切图的工作量，实现社会和谐，原计划除了背景图以外，进度条和thumb的样式都用xml来画。
跟ue大人商量了一下后，得到了一张这样的背景图

[![bg_volume](http://www.4gun.net/wp-content/uploads/2015/02/bg_volume.png)](http://www.4gun.net/wp-content/uploads/2015/02/bg_volume.png)

&nbsp;

在as中稍微操作了一下，改成九宫格图，大概这个样子就差不多了。

[![bg_volume.9](http://www.4gun.net/wp-content/uploads/2015/02/bg_volume.9.png)](http://www.4gun.net/wp-content/uploads/2015/02/bg_volume.9.png)

&nbsp;

然后是进度条，先用ps看一下颜色值，灰色是#959596，青色是#49cca7（为什么ue不直接给不直接给不直接给）
<pre class="lang:default decode:true">&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;layer-list xmlns:android="http://schemas.android.com/apk/res/android"&gt;
    &lt;!-- 背景 --&gt;
    &lt;item android:id="@android:id/background"&gt;
        &lt;shape&gt;
            &lt;corners android:radius="15dip" /&gt;
            &lt;solid android:color="#959596" /&gt;
            &lt;size android:height="1dp" android:width="1dp"/&gt;
        &lt;/shape&gt;
    &lt;/item&gt;

    &lt;!-- 进度条式样 --&gt;
    &lt;item android:id="@android:id/progress"&gt;
        &lt;clip&gt;
            &lt;shape&gt;
                &lt;corners android:radius="15dip" /&gt;
                &lt;solid android:color="#49cca7" /&gt;
            &lt;/shape&gt;
        &lt;/clip&gt;
    &lt;/item&gt;

&lt;/layer-list&gt;</pre>
最后是thumb，一个大白圆（#ffffff）外套一圈1dp的灰色（#7d8288）
<pre class="lang:default decode:true">&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;layer-list xmlns:android="http://schemas.android.com/apk/res/android"&gt;
    &lt;item&gt;
        &lt;shape android:shape="oval" &gt;
            &lt;solid android:color="#ffffff" /&gt;
            &lt;size android:width="14.8dp" android:height="14.8dp" /&gt;
            &lt;stroke android:color="#7d8288" android:width="1dp"/&gt;
        &lt;/shape&gt;
    &lt;/item&gt;
&lt;/layer-list&gt;</pre>
&nbsp;

最后把它们并在一起，组成popup的layout
<pre class="lang:default decode:true">&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
 android:layout_width="wrap_content"
 android:layout_height="wrap_content"
 android:background="@drawable/bg_volume"
 android:gravity="center"
 &gt;
 &lt;com.cooxm.xiaomi.widget.VerticalSeekBar
 android:id="@+id/volumebar"
 android:layout_gravity="center"
 android:layout_width="match_parent"
 android:layout_height="97.8dp"
 android:paddingLeft="8dp"
 android:paddingRight="8dp"
 android:progress="50"
 android:progressDrawable="@drawable/volume_bar"
 android:thumb="@drawable/volume_thumb"
 android:background="@null"
 /&gt;
&lt;/FrameLayout&gt;</pre>
再写成PopupWindow方便随时使用
<pre class="lang:java decode:true">package com.terry.verticalbardemo;

import android.app.ActionBar;
import android.content.Context;
import android.media.AudioManager;
import android.view.LayoutInflater;
import android.view.View;
import android.widget.PopupWindow;
import android.widget.SeekBar;

import com.terry.verticalbardemo.widget.VerticalSeekBar;

/**
 * 声音调整poup window
 * Created by terry on 15-2-11.
 */
public class VolumePopupWindow extends PopupWindow {

    private View view;
    private VerticalSeekBar seekBar;
    private AudioManager audioManager;

    /**
     * 因为本身系统的声音数值的最高值相对较小
     * 为了提高seekbar拖动对声音改变的精度
     * 所以对seekbar的最高值 当前值都乘以一个倍率
     * 对seekbar改变的progress除以一个倍率
     */
    private static final int MULTIPLYING_POWER = 2;

    public VolumePopupWindow(Context context){
        LayoutInflater inflater = LayoutInflater.from(context);
        view = inflater.inflate(R.layout.dialog_volume , null);
        seekBar = (VerticalSeekBar) view.findViewById(R.id.volumebar);
        audioManager = (AudioManager) context.getSystemService(Context.AUDIO_SERVICE);

        seekBar.setMax(audioManager.getStreamMaxVolume(AudioManager.STREAM_MUSIC) * MULTIPLYING_POWER);
        seekBar.setProgress(audioManager.getStreamVolume(AudioManager.STREAM_MUSIC) * MULTIPLYING_POWER);
        seekBar.setOnSeekBarChangeListener(new SeekBar.OnSeekBarChangeListener() {
            @Override
            public void onProgressChanged(SeekBar seekBar, int progress, boolean fromUser) {
                audioManager.setStreamVolume(AudioManager.STREAM_MUSIC, progress / MULTIPLYING_POWER , 0);
            }

            @Override
            public void onStartTrackingTouch(SeekBar seekBar) {

            }

            @Override
            public void onStopTrackingTouch(SeekBar seekBar) {

            }
        });
        setOutsideTouchable(true);
        setContentView(view);
        setWidth(ActionBar.LayoutParams.WRAP_CONTENT);
        setHeight(ActionBar.LayoutParams.WRAP_CONTENT);
        setFocusable(false);
    }

    /**
     * 音量上升
     */
    public void seekUp() {
        if(seekBar.getProgress() &lt; seekBar.getMax()) {
            seekBar.setProgress(seekBar.getProgress() + 1);
            seekBar.onSizeChanged();
        }
    }

    /**
     * 音量下降
     */
    public void seekDown(){
        if(seekBar.getProgress() &gt; 0) {
            seekBar.setProgress(seekBar.getProgress() - 1);
            seekBar.onSizeChanged();
        }
    }

}
</pre>
为了方便外部调用，添加了seekUp()和seekDown()方法用于调整音量。

然后看看效果，前方高能

[![p2](http://www.4gun.net/wp-content/uploads/2015/02/p2.png)](http://www.4gun.net/wp-content/uploads/2015/02/p2.png)

&nbsp;

&nbsp;

&nbsp;

胖....

中间我花了较多时间去研究怎么帮它减肥，具体就不一一列举了，很遗憾没有成功，最后还是要麻烦ue做了两张图（分别是灰色和青色的进度条填充图），图片如下：

[![bg_volume_progress_h.9](http://www.4gun.net/wp-content/uploads/2015/02/bg_volume_progress_h.9.png)](http://www.4gun.net/wp-content/uploads/2015/02/bg_volume_progress_h.9.png)

[![bg_volume_progress_n.9](http://www.4gun.net/wp-content/uploads/2015/02/bg_volume_progress_n.9.png)](http://www.4gun.net/wp-content/uploads/2015/02/bg_volume_progress_n.9.png)

&nbsp;

与背景图一般，做了九宫格的处理，处理完之后如下：

[![bg_volume_progress_n.9](http://www.4gun.net/wp-content/uploads/2015/02/bg_volume_progress_n.91.png)](http://www.4gun.net/wp-content/uploads/2015/02/bg_volume_progress_n.91.png)

[![bg_volume_progress_h.9](http://www.4gun.net/wp-content/uploads/2015/02/bg_volume_progress_h.91.png)](http://www.4gun.net/wp-content/uploads/2015/02/bg_volume_progress_h.91.png)

&nbsp;

注意的点有2个，1是虽然实际显示的是纵向的，但是ue给的图依然要是横向的，所以九宫格图中左右方向实际是纵向SeekBar的上下方向，而九宫格图的上下伸展则是SeekBar的阴影部分（也就是被瘦身的部分），通过这种方式来作出thumb比进度条要宽的情况；2是实际完成后thumb离奇地向左偏移了大概2个像素（如下图），只能确认是thumb本身偏移了，而进度条本身是居中没问题的，但是没有查到偏移的根本原因，时间有限所以直接在thumb的xml中主动添加了2dp的padding来解决。＊补充：在部分模拟器却是没有偏移的，根据实际情况调整吧。
[![p3](http://www.4gun.net/wp-content/uploads/2015/02/p3.png)](http://www.4gun.net/wp-content/uploads/2015/02/p3.png)

&nbsp;

&nbsp;

&nbsp;

&nbsp;

最终完成效果如下图

[![效果图](http://www.4gun.net/wp-content/uploads/2015/02/1.gif)](http://www.4gun.net/wp-content/uploads/2015/02/1.gif)

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

源码：[https://github.com/liqingchang/VerticalBarDemo](https://github.com/liqingchang/VerticalBarDemo)