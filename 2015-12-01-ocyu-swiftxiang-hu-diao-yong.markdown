---
layout: post
title: "OC与swift相互调用"
date: 2015-12-01 11:42:18 +0800
comments: true
tags: [swift,objc,混编,Invalid Swift Support]
keywords: swift,objc,桥连接,Invalid Swift Support 
categories: swift
---
####Swift中使用OC的类声明  -- 实现配置 桥接的头文件
######方式一：自动添加桥接头文件
1. 在一个全新的Swift，利用第一次新建提示的方式自动添加桥接头文件。
2. 点确定这后就会生成一个以<produceName-Bridging-Header.h>的头文件。
3. 在targets->build settings ->Object-C Bridging Header 设为生成的个桥接的头文件即可。
4. 把想要在swift类中调用的OC头文件放使用import "" 写到这个桥接文件中：
```objc
	//  Use this file to import your target's public headers that you would like to expose to Swift.  
	//MixDemo/MixDemo-Bridging-Header.h    
	#import "OCChannel.h"  
```
######方式二：手动添加桥接头文件
同样的，当你知道这个swift搜索头文件的关系后，就不需要再理会这个-Bridging-Header.h的文件了。
完全可以手工建一个并取自己喜欢的名字：
1. 新建一个头文件，名为:OCContainerHeader.h
2. 在targets->build settings ->Object-C Bridging Header 设为生成的个桥接的头文件即可。
3. 把想要在swift类中调用的OC头文件放使用import "" 写到这个桥接文件中：
```objc
//  Use this file to import your target's public headers that you would like to expose to Swift.  	
//MixDemo/MixDemo-Bridging-Header.h    
#import "OCChannel.h"  
```