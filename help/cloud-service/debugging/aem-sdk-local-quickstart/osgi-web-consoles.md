---
title: 使用OSGi Web控制台调试AEM SDK
description: AEM SDK的本地快速入门具有OSGi Web控制台，该控制台在本地AEM运行时中提供各种信息和内部介绍，这些信息和内部介绍对于了解您的应用程序如何被AEM识别以及在中的功能非常有用。
feature: Developer Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5265, 5366, 5267
topic: 开发
role: 开发人员
level: 初学者，中级
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 1%

---


# 使用OSGi Web控制台调试AEM SDK

AEM SDK的本地快速入门具有OSGi Web控制台，该控制台在本地AEM运行时中提供各种信息和内部介绍，这些信息和内部介绍对于了解您的应用程序如何被AEM识别以及在中的功能非常有用。

AEM提供许多OSGi控制台，每个控制台都提供对AEM不同方面的重要洞察，但是，在调试应用程序时，以下控制台通常最有用。

## 捆绑

>[!VIDEO](https://video.tv.adobe.com/v/34335/?quality=12&learn=on)

Bundles控制台是部署到AEM的OSGi捆绑包及其详细信息的目录，以及开始和停止捆绑包的专门功能。

“捆绑套件”控制台位于：

+ “工具”>“操作”>“Web控制台”>“OSGi”>“捆绑包”
+ 或直接在：[http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

单击每个包，提供有助于调试应用程序的详细信息。

+ 正在验证OSGi包
+ 验证OSGi捆绑是否处于活动状态
+ 确定OSGi捆绑包是否存在未满足的导入，阻止其启动

## 组件

>[!VIDEO](https://video.tv.adobe.com/v/34336/?quality=12&learn=on)

组件控制台是部署到AEM的所有OSGi组件的目录，并提供有关它们的所有信息，从它们定义的OSGi组件生命周期到它们可能引用的OSGi服务

组件控制台位于：

+ “工具”>“操作”>“Web控制台”>“OSGi”>“组件”
+ 或直接在：[http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

有助于调试活动的主要方面：

+ 正在验证OSGi包
+ 验证OSGi捆绑是否处于活动状态
+ 确定OSGi捆绑包是否存在未满足的导入，阻止其启动
+ 获取组件的PID，以便在Git中为组件创建OSGi配置
+ 标识绑定到活动OSGi配置的OSGi属性值

## Sling 模型

>[!VIDEO](https://video.tv.adobe.com/v/34337/?quality=12&learn=on)

Sling Models控制台位于：

+ “工具”>“操作”>“Web控制台”>“状态”>“Sling模型”
+ 或直接在：[http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

有助于调试活动的主要方面：

+ 验证Sling模型已注册到正确的资源类型
+ 验证Sling模型可从正确的对象（Resource或SlingHttpRequestServlet）中调整
+ 验证Sling模型导出器已正确注册
