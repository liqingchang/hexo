---
title: Region.Op效果解析
tags:
  - android
  - Region.Op
id: 266
categories:
  - Android
date: 2011-09-05 09:53:10
---

Region.Op有INTERSECT、DIFFERENCE、REPLACE、REVERSE_DIFFERENCE、UNION、XOR七种选择
文档没有详细说明，所以查了点资料并简单的测试了下，权当记忆之用

先看我的测试View的代码
<pre name="code" class="java">
@Override
protected void onDraw(Canvas canvas) {
Path path1 = new Path();
path1.addRect(200, 200, 400, 400, Direction.CW);
canvas.clipPath(path1);
canvas.drawBitmap(red,0, 0, null);

Path path2 = new Path();
path2.addRect(300, 300, 400, 400, Direction.CW);
path2.close();
canvas.clipPath(path1,Region.Op.XOR);
canvas.drawBitmap(blue ,0, 0, null);

}
</pre>

先用Path定出一个红色区域P1，然后再划一片跟红色区域有相交区域的蓝色区域P2，然后分别查看每个参数的效果

INTERSECT，INTERSECT是默认的参数，是指P2实际区域是P1和P2的交集的区域，如图，蓝色部分就是P2实际划分区域，也是两者交集的地方
[![](http://tinone.net/wp-content/uploads/2011/09/INTERSECT.png "INTERSECT")](http://tinone.net/wp-content/uploads/2011/09/INTERSECT.png)

DIFFERENCE，是指P2实际区域是P2与P1不同的地方（P1减去P1和P2的交集）
[![](http://tinone.net/wp-content/uploads/2011/09/DIFFERENCE.png "DIFFERENCE")](http://tinone.net/wp-content/uploads/2011/09/DIFFERENCE.png)

REPLACE，是指P2实际区域就是P2本身，和P1有重合的地方覆盖P1
[![](http://tinone.net/wp-content/uploads/2011/09/REPLACE.png "REPLACE")](http://tinone.net/wp-content/uploads/2011/09/REPLACE.png)

REVERSE_DIFFERENCE，是指P2和P1不重合的区域
[![](http://tinone.net/wp-content/uploads/2011/09/REVERSE_DIFFERENCE.png "REVERSE_DIFFERENCE")](http://tinone.net/wp-content/uploads/2011/09/REVERSE_DIFFERENCE.png)

UNION，P1和P2的并集
[![](http://tinone.net/wp-content/uploads/2011/09/UNION.png "UNION")](http://tinone.net/wp-content/uploads/2011/09/UNION.png)

XOR，P1和P2的并集减去P1和P2的交集
[![](http://tinone.net/wp-content/uploads/2011/09/XOR.png "XOR")](http://tinone.net/wp-content/uploads/2011/09/XOR.png)

以上

技术还不够全面，也不够精，我还要好好加油！