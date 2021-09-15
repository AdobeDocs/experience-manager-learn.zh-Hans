---
title: 使用OSGi Web控制台调试AEM SDK
description: AEM SDK的本地快速启动具有一个OSGi Web控制台，该控制台在本地AEM运行时中提供了各种信息和介绍，有助于了解应用程序如何被识别以及AEM中的功能。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5265, 5366, 5267
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 0929bc1a-376c-4e16-a540-a276fd5af164
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 0%

---

# 使用OSGi Web控制台调试AEM SDK

AEM SDK的本地快速启动具有一个OSGi Web控制台，该控制台在本地AEM运行时中提供了各种信息和介绍，有助于了解应用程序如何被识别以及AEM中的功能。

AEM提供了许多OSGi控制台，每个控制台都对AEM的不同方面提供了重要的分析，但以下在调试应用程序时通常最有用。

## 包

>[!VIDEO](https://video.tv.adobe.com/v/34335/?quality=12&learn=on)

“包”控制台是部署到AEM的OSGi包及其详细信息的目录，以及启动和停止这些包的临时功能。

“包”控制台位于：

+ “工具”>“操作”>“Web控制台”>“OSGi”>“包”
+ 或直接在以下位置：[http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

单击每个包，会提供有助于调试应用程序的详细信息。

+ 验证OSGi包
+ 验证OSGi包是否处于活动状态
+ 确定OSGi包是否具有未满足的导入，从而阻止其启动

## 组件

>[!VIDEO](https://video.tv.adobe.com/v/34336/?quality=12&learn=on)

组件控制台是部署到AEM的所有OSGi组件的目录，提供了有关这些组件的所有信息，从其定义的OSGi组件生命周期，到它们可能引用的OSGi服务

组件控制台位于：

+ “工具”>“操作”>“Web控制台”>“OSGi”>“组件”
+ 或直接在以下位置：[http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

帮助进行调试活动的关键方面：

+ 验证OSGi包
+ 验证OSGi包是否处于活动状态
+ 确定OSGi包是否具有未满足的导入，从而阻止其启动
+ 获取组件的PID，以便在Git中为组件创建OSGi配置
+ 识别绑定到活动OSGi配置的OSGi属性值

## Sling 模型

>[!VIDEO](https://video.tv.adobe.com/v/34337/?quality=12&learn=on)

Sling模型控制台位于：

+ “工具”>“操作”>“Web控制台”>“状态”>“Sling模型”
+ 或直接在以下位置：[http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

帮助进行调试活动的关键方面：

+ 验证Sling模型会注册到正确的资源类型
+ 验证Sling模型可根据正确的对象（资源或SlingHttpRequestServlet）进行调整
+ 验证Sling模型导出程序已正确注册
