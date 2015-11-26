---
layout: post
title: "UIImage的渲染模式"
date: 2015-11-26 16:15:11 +0800
comments: true
categories: 
---

设置UIImage的渲染模式：UIImage.renderingMode

{%codeblock %}
UIImageRenderingModeAutomatic // 根据图片的使用环境和所处的绘图上下文自动调整渲染模式。
UIImageRenderingModeAlwaysOriginal // 始终绘制图片原始状态，不使用Tint Color。
UIImageRenderingModeAlwaysTemplate // 始终根据Tint Color绘制图片，忽略图片的颜色信息。
```
{%endcodeblock%}