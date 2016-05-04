---
title: Windows与VirtualBox Ubuntu共享数据
tags:
  - Ubuntu
  - VirtualBox
  - 共享
id: 140
categories:
  - Code
date: 2011-02-23 00:07:18
---

1\. 安装增强功能包(Guest Additions)   
登录安装在virtualbox中的Ubuntu。然后在VirtualBox的菜单里选择&quot;设备(Devices)&quot; -&gt; &quot;安装增强功能包(Install Guest Additions)&quot;。    
2\. 设置共享文件夹    
重启完成后点击&quot;设备(Devices)&quot; -&gt; 共享文件夹(Shared Folders)/我的是“分配数据空间”菜单，添加一个共享文件夹，数据空间位置自己设定，数据空间名称可以任取一个自己喜欢的，比如&quot;test&quot;。而选项“固定”和“临时”是指该文件夹是否是持久的。    
3\. 创建挂载目录，任何目录都行，但建议在/media或者/mnt目录下创建。比如在在mnt下创建share目录在Ubuntu命令行终端下输入：    
sudo mkdir /mnt/share    
4、挂载共享文件夹    
sudo mount -t vboxsf test /mnt/share    
其中&quot;test&quot;是之前创建的共享文件夹的名字。    
假如您不想每一次都手动挂载，可以在用sudo gedit /etc/fstab在/etc/fstab中添加一项    
test /mnt/share vboxsf rw,gid=100,uid=1000,auto 0 0    
这样就能够自动挂载了。    
5\. 卸载的话使用下面的命令    
sudo umount -f /mnt/share