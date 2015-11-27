---
layout: post
title: "Xcode&amp;Jenkins持续集成的几种实现方式"
date: 2015-09-18 23:52:06 +0800
comments: true
tags: [持续集成,TDD,BDD]
keywords: 持续集成,TDD,BDD
categories: 持续集成
---
####CI服务器

写到这儿，对于iOS开发者来说，需要准备好：

- 一个比较容易获取的源代码仓库(包含源代码)
- 一套自动化构建脚本
- 一系列围绕构建的可执行测试  

接下来就需要一个CI服务器来根据源代码的变更触发构建，监控测试结果。
  
目前，业界比较流行的，支持iOS构建的CI服务器有

- [Travis CI](https://travis-ci.org)：是一个免费的云服务平台，基本上支持所有目前主流的语言，Object-C自然也在其中，但是只支持github极大的限制了其应用场景。目前国内无法访问，[详见](http://www.infoq.com/cn/articles/build-ios-continuous-integration-platform-part3)
- **Jenkins**：经过多年的发展，其活跃的社区和丰富的插件让其成为了业界最受欢迎的CI服务器。通过使用Xcode插件，可以非常方便在Jenkins中运行iOS项目的构建脚本。

#### xcode 持续集成的实现
[Setting Up Xcode Server](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/xcode_guide-continuous_integration/adopt_continuous_integration.html#//apple_ref/doc/uid/TP40013292-CH3-SW1)


####安装jenkins使用配置：  

1. 下载：http://mirrors.jenkins-ci.org/war/lastest/jenkins.war  
2. 运行命令行：  

	```
 	 nohup java -jar ~/Downloads/jenkins.war —httpPort=8081 —ajp13Port=8010 > /tmp/jenkins.log 2>&1 &
	```  
3. 写入启动文件中，起别名  

	```
	vi /Users/(username)/.bash_profile  
	输入:alias jenkins="nohup java -jar ~/Downloads/SVNRepos/jenkins.war --httpPort=8081 --ajp13Port=8010 > /tmp/jenkins.log 2>&1 &”  
	```  
4. 启动时，在命令行中输入：**`jenkins`** 回车  即可启动
5. 访问：http://127.0.0.1:8081/
6. 重启：http://[jenkins-server]/[command] exit推出，restart重启，reload重载。

####方法二：
安装jenkins还是使用brew

brew install jenkins
安装好之后，可以通过使用命令行启动

	java -jar /usr/local/opt/jenkins/libexec/jenkins.war
如果想**开机自动启动**，需要先执行以下命令，创建启动项：

	ln -sfv /usr/local/opt/jenkins/*.plist ~/Library/LaunchAgents
可以编辑一下~/Library/LaunchAgents/homebrew.mxcl.jenkins.plist这个文件

	open ~/Library/LaunchAgents/homebrew.mxcl.jenkins.plist
	
具体内容：

	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
	<plist version="1.0">
	  <dict>
	    <key>Label</key>
	    <string>homebrew.mxcl.jenkins</string>
	    <key>ProgramArguments</key>
	    <array>
	      <string>/usr/bin/java</string>
	      <string>-Dmail.smtp.starttls.enable=true</string>
	      <string>-jar</string>
	      <string>/usr/local/opt/jenkins/libexec/jenkins.war</string>
	      <string>--httpListenAddress=127.0.0.1</string>
	      <string>--httpPort=8088</string>
	    </array>
	    <key>RunAtLoad</key>
	    <true/>
	  </dict>
	</plist>
	
想要让局域网都可以访问或修改端口号，需要把—httpListenAddress=127.0.0.1改成自己的局域网IP  

手动启动启动项可以执行,制作替身：

	launchctl load ~/Library/LaunchAgents/homebrew.mxcl.jenkins.plist  
之后用浏览器就可以访问 **http://localhost:8088/** 来登录jenkins了

####方法三：
使用tomcat
制作替身：

	cd ~/Downloads/soft/Tomcat/
	ln -sfv apache-tomcat-8.0.27 tomcat
	
将jenkins.war拷贝到 $tomcat/webapp下面。  
	
	 $tomcat/bin/start.sh  
用浏览器打开 **localhost:8080/jenkins** tomcat默认端口号为8080，就可以看到 jenkin运行了。

####自动化构建和依赖管理[参考](http://www.infoq.com/cn/articles/build-ios-continuous-integration-platform-part1/)
作为以GUI和命令行操作结合的完美性著称的苹果公司来说，当然也不会忘记为自己的封闭的iOS系统提供开发环境下命令行编译工具：xcodebuild
在介绍xcodebuild之前，需要先弄清楚一些在XCode环境下的一些概念【4】：

- **Workspace**：简单来说，Workspace就是一个容器，在该容器中可以存放多个你创建的Xcode Project， 以及其他的项目中需要使用到的文件。使用Workspace的好处有，1),扩展项目的可视域，即可以在多个项目之间跳转，重构，一个项目可以使用另一个项目的输出。Workspace会负责各个Project之间提供各种相互依赖的关系;2),多个项目之间共享Build目录。
- **Project**：指一个项目，该项目会负责管理生成一个或者多个软件产品的全部文件和配置，一个Project可以包含多个Target。
- **Target**：一个Target是指在一个Project中构建的一个产品，它包含了构建该产品的所有文件，以及如何构建该产品的配置。
- **Scheme**：一个定义好构建过程的Target成为一个Scheme。可在Scheme中定义的Target的构建过程有：Build/Run/Test/Profile/Analyze/Archive
- **BuildSetting**：配置产品的Build设置，比方说，使用哪个Architectures？使用哪个版本的SDK？。在Xcode Project中，有Project级别的Build Setting，也有Target级别的Build Setting。Build一个产品时一定是针对某个Target的，因此，XCode中总是优先选择Target的Build Setting，如果Target没有配置，则会使用Project的Build Setting。

xcodebuild就是用了构建产品的命令行工具，其用法可以归结为3个部分：

- 可构建的对象
- 构建行为
- 一些其他的辅助命令

可以构建的对象有，默认情况下会运行project下的第一个target：

- workspace：必须和“-scheme”一起使用，构建该workspace下的一个scheme。
- project：当根目录下有多个Project的时候，必须使用“-project”指定project，然后会运行
- target：构建某个Target
- scheme：和“-workspace”一起使用，指定构建的scheme。
- ……

构建行为包括：

- clean:清除build目录下的
- build: 构建
- test: 测试某个scheme，必须和"-scheme"一起使用
- archive:打包，必须和“-scheme”一起使用
- ……
辅助命令包括：

- -sdk：指定构建使用的SDK
- -list：列出当前项目下所有的Target和scheme。
- -version：版本信息
- …...
关于xcodebuild更多详细的命令行请参见：[点击](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/xcodebuild.1.html)

xcodebuild的主要缺陷：

- 其脚本输出的可读性极差，
- 只能要么完整的运行一个target或者scheme，要么全部不运行。不能指定运行Target中特定的测试。

**安装xctool** 

xctool的安装非常简单，只需要clone xctool的repository到项目根目录就可以使用， 如果你的机器上安装有Homebrew，可以通过“brew install xctool”命令直接安装。（**注意：使用xctool前一定要首先确认xcodebuild已安装且能正确工作**）。

**用法**

关于xctool的用法就更加人性化了，几乎可以重用所有的xcodebuild的指令，配置。只需要注意一下几点：

- xctool不支持target构建，只能使用scheme构建。
- 支持“-only”指令运行指定的测试。
- 支持多种格式的build报告。
例子：
```ruby
path/to/xctool.sh 
  -workspaceYourWorkspace.xcworkspace
  -schemeYourScheme
test -only SomeTestTarget:SomeTestClass/testSomeMethod
```

####自动化部署

这儿的想谈的“部署”不是传统意义上的直接部署到产品环境的部署，而是指如何把最新版本的应用快速的部署到测试用户的机器上以收集反馈，或者做一些探索性的测试。  

在我写第一个iOS应用的时候，我想把应用安装到多个机器上测试的时候，需要非常繁琐的步骤：

- 需要申请到苹果开发者账号，获得开发者证书。
- 需要在苹果的开发者网站上注册我想使用的设备。
- 使用开发者证书打包应用，使用Ad-HOC部署模式，生成ipa文件。
- 通过ipa文件把应用安装到iTunes上。
- 通过iTunes把应用同步到多台测试机器上。

如果是测试机器在多个地理位置的时候，还需要把ipa文件发送到对应的地点，每个地点都需要重复的做第4，5步。 这样一个繁琐，且低效的过程让开发者非常痛苦，直到TestFlight的出现。

####TestFlight

TestFlight：就是一个专门解决上面提到的痛点的云服务方案，它可以帮助开发者：

- 轻松采集测试用户的UDID和iOS 版本、硬件版本，并发送给开发者。
- 实时反馈应用是否成功安装到测试机器
- 轻松部署最新版本应用到测试用机上。
- 开发者可以灵活选择部署哪个版本到哪部分测试机器上。

使用使用Test Flight服务非常简单，只需要到Test Flight注册一个账号。然后把链接发送给测试设备，测试设备只要打开该链接，并授权给Test Flight，在Test Flight的设备中心就可以看到这些设备。