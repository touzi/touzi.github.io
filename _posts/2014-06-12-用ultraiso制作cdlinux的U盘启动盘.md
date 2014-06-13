---
date: 2014/6/12 20:22:45  
layout: post
title: 用ultraiso制作cdlinux的U盘启动盘
thread: 1
categories: linux
tags:  网络技术 系统
---

#用ultraiso制作cdlinux的U盘启动盘
----

###准备工作：

1. cdlinux (我选用的标准版的)http://cdlinux.info/wiki/doku.php/download/latest
2. ultraiso 这个自己下吧，网盘没有，网上还是挺多的
3. bootice http://pan.baidu.com/s/1eQvHCUu
4. grub4dos-0.4.4
5. 格式化的u盘
6. 改动的menu.lst http://pan.baidu.com/s/1sjPhyUh


一、启动ultraiso软件，文件->打开（cdlinux的镜像文件.ISO ）->启动->写入硬盘镜像 （写入方式USB-HDD+）

二、启动bootice 物理磁盘处理->主引导记录->选择Grub4DOS->安装/配置

三、将grub4dos-0.4.4复制U盘根目录下。

四、将grub4dos-0.4.4目录下的grldr复制到U盘根目录下

五、将改动的menu.lst复制到U盘根目录下

六、重启电脑，进入BIOS，选择从U盘启动。

ｐｓ：改动的ｍenu.lst 就是在grub4dos-0.4.4目录下的ｍenu.lst的最后加如下代码：

	title CDLINUX
	find –set-root /CDLINUX/BZIMAGE.
	kernel /CDLINUX/BZIMAGE. CDL_DEV=LABEL=CDLINUX CDL_LANG=zh_CN.UTF-8
	initrd /CDLINUX/INITRD.
	boot

---------
end

2014/6/13 10:48:49 
