---
title: 使用OSGi Web控制台调试AEM SDK
description: AEM SDK的本地快速入门具有OSGi Web控制台，该控制台为本地AEM运行时提供各种信息和说明，对于了解AEM如何识别和运行您的应用程序非常有用。
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
kt: 5265, 5366, 5267
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 0929bc1a-376c-4e16-a540-a276fd5af164
duration: 516
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 0%

---

# 使用OSGi Web控制台调试AEM SDK

AEM SDK的本地快速入门具有OSGi Web控制台，该控制台为本地AEM运行时提供各种信息和说明，对于了解AEM如何识别和运行您的应用程序非常有用。

AEM提供了许多OSGi控制台，每个控制台都提供了有关AEM各个方面的关键见解，但是，以下内容通常在调试应用程序时最有用。

## 包

>[!VIDEO](https://video.tv.adobe.com/v/34335?quality=12&learn=on)

捆绑包控制台是OSGi捆绑包的目录，其详细信息部署到AEM，并且具有启动和停止这些捆绑包的临时功能。

捆绑包控制台位于：

+ “工具” > “操作” > “Web控制台” > “OSGi” > “包”
+ 或直接访问： [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

单击每个捆绑包，可提供有助于调试应用程序的详细信息。

+ 验证是否存在OSGi捆绑包
+ 验证OSGi捆绑包是否有效
+ 确定OSGi捆绑包是否有不满足的导入阻止其启动

## 组件

>[!VIDEO](https://video.tv.adobe.com/v/34336?quality=12&learn=on)

组件控制台是部署到AEM的所有OSGi组件的目录，提供了有关这些组件的所有信息，从其定义的OSGi组件生命周期到他们可以引用的OSGi服务

“组件”控制台位于：

+ “工具” > “操作” > “Web控制台” > “OSGi” > “组件”
+ 或直接访问： [http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

有助于调试活动的关键方面：

+ 验证是否存在OSGi捆绑包
+ 验证OSGi捆绑包是否有效
+ 确定OSGi捆绑包是否有不满足的导入阻止其启动
+ 获取组件的PID，以便在Git中为其创建OSGi配置
+ 标识绑定到活动OSGi配置的OSGi属性值

## Sling 模型

>[!VIDEO](https://video.tv.adobe.com/v/34337?quality=12&learn=on)

Sling模型控制台位于：

+ 工具>操作> Web控制台>状态> Sling模型
+ 或直接访问： [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

有助于调试活动的关键方面：

+ 验证Sling模型是否已注册到正确的资源类型
+ 验证Sling模型是否可从正确的对象（资源或SlingHttpRequestServlet）中进行调整
+ 验证Sling模型导出程序已正确注册
