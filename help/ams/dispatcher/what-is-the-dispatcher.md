---
title: 什么是“Dispatcher”
description: 了解Dispatcher到底是什么。
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 96c8dd09-e0a7-4abc-b04b-a805aaa67502
duration: 73
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 2%

---

# 什么是“Dispatcher”

[目录](./overview.md)

首先简要说明AEM Dispatcher的具体含义。

## Apache Web Server

从Linux服务器上的基本Apache Web Server安装开始。

Apache服务器功能的基本说明：

- 遵循简单规则从其静态文档目录(`DocumentRoot`)通过HTTP(s)协议提供文件
- 存储在默认位置(`/var/www/html`)中的文件在请求时匹配，并在请求客户端的浏览器中呈现




## AEM特定模块文件(`mod_dispatcher.so`)

然后，向Apache Web Server添加一个称为Dispatcher模块的插件

有关AdobeAEM Dispatcher模块功用的基本说明：

- 增强默认文件处理程序
- 过滤出错误请求/保护AEM的软肚子/端点
- 存在多个呈现器时的负载平衡
- 允许使用活动缓存目录/支持刷新停滞文件
- 它是所有AMS安装的前门，它将网站和资产提供给客户端的浏览器
- 它以比AEM服务器单独完成速度快得多的速度缓存要重新提供的请求
- 更多……

## Web流量工作流程

了解哪些部分一起安装在一起以构建基本的Dispatcher服务器，可使您了解Adobe管理器服务配置的基本Web流量工作流。
这应该有助于您了解它在向AEM内容的访客提供内容的系统链中发挥什么作用。

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

[下一个 — >基本文件布局](./basic-file-layout.md)
