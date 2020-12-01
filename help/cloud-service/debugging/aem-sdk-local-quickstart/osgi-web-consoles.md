---
title: 使用OSGi Web控制台调试AEM SDK
description: AEM SDK的本地快速启动具有一个OSGi Web控制台，它向本地AEM运行时提供各种信息和介绍，这些信息和介绍有助于了解您的应用程序如何被识别以及AEM中的功能。
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5265, 5366, 5267
translation-type: tm+mt
source-git-commit: a3d3612713decefb5c7e1cf5b2d4d21afff0a2f5
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 0%

---


# 使用OSGi Web控制台调试AEM SDK

AEM SDK的本地快速启动具有一个OSGi Web控制台，它向本地AEM运行时提供各种信息和介绍，这些信息和介绍有助于了解您的应用程序如何被识别以及AEM中的功能。

AEM提供许多OSGi控制台，每个控制台都提供对AEM不同方面的重要见解，但在调试应用程序时，以下控制台通常最有用。

## 捆绑

>[!VIDEO](https://video.tv.adobe.com/v/34335/?quality=12&learn=on)

Bundles控制台是部署到AEM的OSGi捆绑包及其详细信息的目录，以及开始和停止捆绑包的专门功能。

Bundles控制台位于：

+ “工具”>“操作”>“Web控制台”>“OSGi”>“捆绑包”
+ 或直接在：[http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

单击每个包，提供有助于调试应用程序的详细信息。

+ 验证OSGi捆绑包
+ 验证OSGi捆绑是否处于活动状态
+ 确定OSGi捆绑是否存在未满足的导入，阻止其启动

## 组件

>[!VIDEO](https://video.tv.adobe.com/v/34336/?quality=12&learn=on)

组件控制台是部署到AEM的所有OSGi组件的目录，它提供有关这些组件的所有信息，从它们定义的OSGi组件生命周期到它们可能引用的OSGi服务

组件控制台位于：

+ “工具”>“操作”>“Web控制台”>“OSGi”>“组件”
+ 或直接在：[http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

有助于调试活动的关键方面：

+ 验证OSGi捆绑包
+ 验证OSGi捆绑是否处于活动状态
+ 确定OSGi捆绑是否存在未满足的导入，阻止其启动
+ 获取组件的PID，以便在Git中为组件创建OSGi配置
+ 标识绑定到活动OSGi配置的OSGi属性值

## Sling Models

>[!VIDEO](https://video.tv.adobe.com/v/34337/?quality=12&learn=on)

Sling Models控制台位于：

+ “工具”>“操作”>“Web控制台”>“状态”>“Sling模型”
+ 或直接在：[http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

有助于调试活动的关键方面：

+ 验证Sling模型已注册到正确的资源类型
+ 验证Sling模型可以根据正确的对象（资源或SlingHttpRequestServlet）进行调整
+ 验证Sling模型导出程序已正确注册
