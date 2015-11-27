---
layout: post
title: "Swift入门"
date: 2015-11-27 17:18:21 +0800
comments: true
categories: 
---
1. 在一个全新的Swift，利用第一次新建提示的方式自动添加桥接头文件。
2. 点确定这后就会生成一个以<produceName-Bridging-Header.h>的头文件。
3. 在targets->build settings ->Object-C Bridging Header 设为生成的个桥接的头文件即可。
4. 把想要在swift类中调用的OC头文件放使用import "" 写到这个桥接文件中：
	1.	//  Use this file to import your target's public headers that you would like to expose to Swift.  
	2.	//MixDemo/MixDemo-Bridging-Header.h    
	4.	#import "OCChannel.h"  