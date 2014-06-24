---
date: 2014/6/24 20:37
layout: post
title: OS X Mavericks制作U盘启动盘
thread: 1
categories: Mac技巧
tags: OS X mavericks Mac 系统 苹果

---

###说明

由于10.9 的安装盘跟之前的版本制作方式不一样，所以很多人在10.9的时候还用原来的方式制作U盘启动，结果导致开机按住option键的时候无法找到启动盘。

###开始制作

####不要问为什么，先按照我说的做，拯救了Mac在来讨论为什么

#####1. 显示隐藏文件。

在终端中执行以下命令：

	defaults write com.apple.finder AppleShowAllFiles -bool TRUE;killall Finder

#####2. 挂载 InstallESD.dmg 。

右键 “Install OS X 10.9.app” 选择 “显示包内容” ，进入 Contents/SharedSupport 中找到 InstallESD.dmg 并双击挂载。

#####3.将 BaseSystem.dmg 恢复到 U 盘。

打开磁盘工具，将挂载的 OS X Install ESD 目录中的隐藏文件 BaseSystem.dmg 拖入磁盘工具的源磁盘，目的磁盘选择 U 盘，点击恢复，恢复过程大概用时 10 分钟。

#####4.替换 U 盘中 Packages 文件。

打开 U 盘（此时 U 盘名为 OS X Base System ），进入 System/Installation 目录，将带有快捷方式剪头的 Packages 文件删除。再将挂载的 OS X Install ESD 目录中的所有文件(包括：BaseSystem.chunklist、BaseSystem.dmg、Pakages，大概 5.28G )都拷贝到 U 盘中的 System/Installation 目录下。

#####5.至此启动 U 盘制作完成。

将 U 盘插入电脑，按住 option 键开机，然后在选择启动磁盘的界面中选择 U 盘启动。


#####注：如果想取消对隐藏文件的显示，终端中执行以下命令：

	defaults write com.apple.finder AppleShowAllFiles -bool FALSE;killall Finder
	
----

end 

2014年6月24日21:54
