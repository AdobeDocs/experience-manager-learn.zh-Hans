---
title: 什么是“调度程序”
description: 了解Dispatcher的实际含义。
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 829ad9733b4326c79b9b574b13b1d4c691abf877
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 2%

---


# 什么是“调度程序”

[目录](./overview.md)

从AEM Dispatcher的基本描述开始。

## Apache Web Server

从在Linux服务器上安装基本的Apache Web Server开始。

Apache服务器功能的基本说明：

- 遵循通过HTTP协议从其静态文档目录(`DocumentRoot`)
- 存储在默认位置(`/var/www/html`)会在请求时进行匹配，并在请求客户端的浏览器中呈现




## AEM特定模块文件(`mod_dispatcher.so`)

然后，向Apache Web Server中添加一个名为Dispatcher模块的插件

AdobeAEM Dispatcher模块功能的基本说明：

- 增强默认文件处理程序
- 过滤出错误请求/保护AEM软端点/端点
- 如果存在多个渲染器，则负载平衡
- 允许生命缓存目录/支持刷新停滞文件
- 它是所有AMS安装的前门，可将网站和资产交付到客户端的浏览器
- 它以比AEM服务器单独完成的速度快得多的速度缓存重新提供服务的请求
- 更多……

## Web流量工作流

了解哪些部分已安装在一起以构建基本的Dispatcher服务器，这将使您了解Adobe管理器服务配置的基本Web流量工作流程。
这应该有助于您了解它在为AEM内容的访客提供内容的系统链中所起的作用。

<b>提供已缓存的内容</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if found 
                → return request 
                    → End User
```

<b>从AEM提供新内容</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if NOT found 
                → requests content from publisher 
                    → publisher sends content 
                        → Dispatcher adds content to cache and replies 
                            → End User
```

<b>内容发布/更改</b>

```
AEM Author User activates content 
    → Triggers content to be replicated to Publisher 
        → Publisher gets content and triggers the flush request to Dispatcher 
            → Dispatcher invalidates changed content 
            * Next request for that content will request fresh copy from publisher *
```

[下一步 — >基本文件布局](./basic-file-layout.md)