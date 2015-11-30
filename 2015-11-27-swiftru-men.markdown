---
layout: post
publiced: false
title: "Swift入门"
date: 2015-11-27 17:18:21 +0800
comments: true
tags:[swift,语法]
keywords: Swift,语法
categories: Swift
---

####背景
Apple基于已有的编译器、调试器、框架作为其基础架构。通过ARC(Automatic Reference Counting，自动引用计数)来简化内存管理。我们的框架栈则一直基于Cocoa，且Objective-C进化支持了块、collection literal和模块，允许现代语言的框架无需深入即可使用。
(by gashero)感谢这些基础工作，才使得可以在Apple软件开发中引入新的编程语言Swift。

####swift有点

编译器是按照性能优化的，而语言是为开发优化的

Swift采用了Objective-C的命名参数和动态对象模型。提供了对Cocoa框架和mix-and-match的互操作性。基于这些基础，Swift引入了很多新功能和结合面向过程和面向对象的功能。
Swift对新的程序员也是友好的：

1. 它是工业级品质的系统编程语言，却又像脚本语言一样的友好。
2. 它支持playground，允许程序员实验一段Swift代码功能并立即看到结果，而无需麻烦的构建和运行一个应用。
Swift集成了现代编程语言思想，以及Apple工程文化的智慧，编译器是按照性能优化的，而语言是为开发优化的，无需互相折中。

####swift语法
Playground允许你编辑代码并立即看到结果,可以从”Hello, world”开始学起并过渡到整个系统。
在Xcode的playground中打开:
```objc
println("Hello, world")
```

在Swift，这就是完整的程序:
1. 无需导入(import)输入输出和字符串处理的系统库。
2. 全局范围的代码就是用于程序的入口，所以你无需编写一个 main() 函数。也无需在每个语句后写分号。

所有这些使得Swift成为Apple软件开发者创新的源泉。

######简单值  -- 使用 let 来定义常量， var 定义变量
提供一个值就可以创建常量或变量，并让编译器推断其类型,一个常量或变量必须与赋值时拥有相同的类型。因此你不用严格定义类型。
常量定义类似于函数式编程语言中的变量,常量的值无需在编译时指定，但是至少要赋值一次,赋值后就无法修改。
```
var myVariable = 42
myVariable = 50
let myConstant = 42
```
在上面例子中，编译其会推断myVariable是一个整数类型，因为其初始化值就是个整数。

######数组和字典的用法
1.声明并初始化
```objc
let emptyArray = String[]()
let emptyDictionary = Dictionary<String, Float>()

shoppingList = [] //去购物并买些东西 
```
如果数组类型无法推断，你可以写空的数组为 “[]” 和空的字典为 “[:]“，





