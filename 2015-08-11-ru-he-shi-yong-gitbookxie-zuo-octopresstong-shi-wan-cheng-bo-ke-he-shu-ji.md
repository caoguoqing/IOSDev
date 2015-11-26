---
layout: post
title: "如何使用gitBook协作Octopress同时完成博客和书籍"
date: 2015-08-11 14:41:40 +0800
comments: true
tags: [octopress,blog,github,mou,ruby]
keywords: boys,octopress,技术
categories: Octopress
---
使用GitBook（**版本限于4.0之前的版本**） 来编写Octopress博客的步骤：  
1. ```cd ~/MyBlog```  
2. **`rake new_post['文章名']`**或 **`rake new_page['404']`**新建md文档.  
3. **`mv *.markdown *.md`** mv命令修改后缀为md，便于gitbook在Preview website识别该文档。  
4. 配置SUMMARY.md 关联 gitbook，通过目录访问Octopress文档。  
5. 打开gitbook客户端，对新建的文档进行编写即可。

更新：gitbook 4.2.2之后，默认将rake new_post生成的文件目录导入到~/gitbook目录中，至此，以上方法作废了。