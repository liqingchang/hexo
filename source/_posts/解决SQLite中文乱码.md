---
title: 解决SQLite中文乱码
tags:
  - android
  - sqlite
  - 中文乱码
id: 214
categories:
  - Android
date: 2011-06-21 09:38:23
---

有朋友反映SQLite中文乱码，所以找了下解决方案

先看以下查询数据方法
  <table cellspacing="0" cellpadding="2" width="400" border="0"><tbody>     <tr>       <td valign="top" width="400">//查询数据          
public String query(SQLiteHelper sQLiteHelper){           
&#160;&#160;&#160; String result=&quot;&quot;;           
&#160;&#160;&#160; SQLiteDatabase db = sQLiteHelper.getWritableDatabase();           
&#160;&#160;&#160; Cursor cursor = db.query(&quot;students&quot;, null, &quot;id='19'&quot;, null, null, null, null, null);           
&#160;&#160;&#160; cursor.moveToFirst();           
&#160;&#160;&#160; result = cursor.getString(cursor.getColumnIndex(&quot;name&quot;));           
&#160;&#160;&#160; return result;           
}</td>     </tr>   </tbody></table>  

结果果然乱码了

[![image](http://tinone.net/wp-content/uploads/2011/06/image_thumb.png "image")](http://tinone.net/wp-content/uploads/2011/06/image.png) 

于是用String对象创建时的转码来创建数据，代码如下
  <table cellspacing="0" cellpadding="2" width="400" border="0"><tbody>     <tr>       <td valign="top" width="400">         

&#160;&#160;&#160; //查询数据            
&#160;&#160;&#160; public String query(SQLiteHelper sQLiteHelper){             
&#160;&#160;&#160;&#160;&#160;&#160;&#160; String result=&quot;&quot;;             
&#160;&#160;&#160;&#160;&#160;&#160;&#160; SQLiteDatabase db = sQLiteHelper.getWritableDatabase();             
&#160;&#160;&#160;&#160;&#160;&#160;&#160; Cursor cursor = db.query(&quot;students&quot;, null, &quot;id='19'&quot;, null, null, null, null, null);             
&#160;&#160;&#160;&#160;&#160;&#160;&#160; cursor.moveToFirst();             
&#160;&#160;&#160;&#160;&#160;&#160;&#160; byte[] name = cursor.getBlob(cursor.getColumnIndex(&quot;name&quot;));             
&#160;&#160;&#160;&#160;&#160;&#160;&#160; try {             
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; result&#160; = new String(name,&quot;GBK&quot;);             
&#160;&#160;&#160;&#160;&#160;&#160;&#160; } catch (UnsupportedEncodingException e) {             
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; // TODO Auto-generated catch block             
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160; e.printStackTrace();             
&#160;&#160;&#160;&#160;&#160;&#160;&#160; }             
&#160;&#160;&#160;&#160;&#160;&#160;&#160; return result;             
&#160;&#160;&#160; }
       </td>     </tr>   </tbody></table>  

成功输出了中文

[![image](http://tinone.net/wp-content/uploads/2011/06/image_thumb1.png "image")](http://tinone.net/wp-content/uploads/2011/06/image1.png)

由此解决了SQLite中文乱码的问题