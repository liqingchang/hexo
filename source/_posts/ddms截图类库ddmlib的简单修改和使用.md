---
title: ddms截图类库ddmlib的简单修改和使用
tags:
  - android
  - ddmlib
  - ddms
  - 截图
id: 330
categories:
  - Android
  - Code
date: 2012-02-10 17:16:09
---

像豌豆荚、91助手这样的PC端连接Android的软件，在PC端获取截图的方法应该都是使用高效的ddms截图方式，于是就需要使用ddmlib这个库。
为了方便使用，我在ddmlib库的基础上增加了一个ScreenShot的类，提供一些简单的方法用于截图。
如果开发语言是C++，则可以jni调用，做起来也很方便

ScreenShot.java
<pre lang="java" line="1">
package com.android.ddmlib;

import java.awt.image.BufferedImage;
import java.awt.image.RenderedImage;
import java.io.File;
import java.io.IOException;

import javax.imageio.ImageIO;

public class ScreenShot {

	public IDevice device ;

	/**
	 * 构造函数，默认获取第一个设备
	 */
	public ScreenShot(){
		AndroidDebugBridge.init(false); //
		device = this.getDevice(0);
	}

	/**
	 * 构造函数，指定设备序号
	 * @param deviceIndex 设备序号
	 */
	public ScreenShot(int deviceIndex){
		AndroidDebugBridge.init(false); //
		device = this.getDevice(deviceIndex);
	}

	/**
	 * 直接抓取屏幕数据
	 * @return 屏幕数据
	 */
	public RawImage getScreenShot(){
		RawImage rawScreen = null;
		if(device!=null){
			try {
				rawScreen = device.getScreenshot();
			} catch (TimeoutException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			} catch (AdbCommandRejectedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			} catch (IOException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}else{
			System.err.print("没有找到设备");
		}
		return rawScreen;
	}

	/**
	 * 获取图片byte[]数据
	 * @return 图片byte[]数据
	 */
	public byte[] getScreenShotByteData(){
		RawImage rawScreen = getScreenShot();
		if(rawScreen != null){
			return rawScreen.data;
		}
		return null;
	}

	/**
	 * 抓取图片并保存到指定路径
	 * @param path 文件路径
	 * @param fileName 文件名
	 */
	public void getScreenShot(String path,String fileName){
		RawImage rawScreen = getScreenShot();
		if(rawScreen!=null){
			Boolean landscape = false;
			int width2 = landscape ? rawScreen.height : rawScreen.width;
			int height2 = landscape ? rawScreen.width : rawScreen.height;
			BufferedImage image = new BufferedImage(width2, height2,
					BufferedImage.TYPE_INT_RGB);
			if (image.getHeight() != height2 || image.getWidth() != width2) {
				image = new BufferedImage(width2, height2,
						BufferedImage.TYPE_INT_RGB);
			}
			int index = 0;
			int indexInc = rawScreen.bpp >> 3;
			for (int y = 0; y < rawScreen.height; y++) {
				for (int x = 0; x < rawScreen.width; x++, index += indexInc) {
					int value = rawScreen.getARGB(index);
					if (landscape)
						image.setRGB(y, rawScreen.width - x - 1, value);
					else
						image.setRGB(x, y, value);
				}
			}
			try {
				ImageIO.write((RenderedImage) image, "PNG", new File(path + "/" + fileName + ".png"));
			} catch (IOException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
	}

	/**
	 * 获取得到device对象
	 * @param index 设备序号
	 * @return 指定设备device对象
	 */
	private IDevice getDevice(int index) {
		IDevice device = null;
		AndroidDebugBridge bridge = AndroidDebugBridge
				.createBridge("adb", true);// 如果代码有问题请查看API，修改此处的参数值试一下
		waitDevicesList(bridge);
		IDevice devices[] = bridge.getDevices();
		if(devices.length < index){
			//没有检测到第index个设备
			System.err.print("没有检测到第" + index + "个设备");
		}else{
			device = devices[index-1];
		}
		return device;
	}

	/**
	 * 等待查找device
	 * @param bridge
	 */
	private void waitDevicesList(AndroidDebugBridge bridge) {
		int count = 0;
		while (bridge.hasInitialDeviceList() == false) {
			try {
				Thread.sleep(500);
				count++;
			} catch (InterruptedException e) {
			}
			if (count > 60) {
				System.err.print("等待获取设备超时");
				break;
			}
		}
	}
}
</pre>

使用的话很简单，这里有一个简单的例子（注意这是计算机上的截取Android屏幕的工具，并不是Android上截屏的工具），创建一个Java项目就行了
<pre lang="java" line="1">
public class HelloShot {
	public static void main(String[] args) {
		ScreenShot screenShot = new ScreenShot();
		screenShot.getScreenShot("d:/", "abcd");
	}
}
</pre>

附一个改动后的dbmlib.jar和dbmlib的源码下载
[ddmlib源码](http://tinone.net/wp-content/uploads/2012/02/ddmlib.rar)
[ddmlib](http://tinone.net/wp-content/uploads/2012/02/ddmlib1.rar)