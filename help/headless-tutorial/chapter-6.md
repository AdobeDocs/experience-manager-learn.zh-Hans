---
title: AEM无头入门——第6章——将AEM发布中的内容公开为JSON
description: AEM无头教程的第6章介绍了如何确保AEM发布中包含所有必要的包、配置和内容，以允许从移动应用程序进行使用。
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '468'
ht-degree: 0%

---


# 第6章——公开AEM发布中的内容以供投放

AEM无头教程的第6章介绍了如何确保AEM发布中包含所有必要的包、配置和内容，以允许移动应用程序使用。

## 发布AEM Content Services的内容

为通过AEM Content Services驱动事件而创建的配置和内容必须发布到AEM Publish，这样移动应用程序才能访问它。

由于AEM Content Services是从配置（内容片段模型、可编辑模板）、资产（内容片段、图像）和页面构建的，所有这些部分都会自动享受AEM内容管理功能，包括：

* 用于审阅和处理的工作流
* 和激活/取消激活，以从AEM Publish的AEM Content Services端点推送和拉取内容

1. 确保使 **[!DNL WKND Mobile]用包** Manager在AEM Publish上安装 [了第1章](./chapter-1.md#wknd-mobile-application-packages)中列出的应用程 ****序包。
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. 发布可编 **[!DNL WKND Mobile Events API]辑模板**
   1. 导航到 **[!UICONTROL AEM]>[!UICONTROL Tools]>[!UICONTROL General]>[!UICONTROL Templates]>[!DNL WKND Mobile]**
   1. 选择模 **[!DNL Event API]** 板
   1. 点 **[!UICONTROL 按顶部]** 操作栏中的发布
   1. 发布模 **板****和所有引** 用（内容策略、内容策略映射和模板）

1. 发布内 **[!DNL WKND Mobile Events]容片段**。

请注意，这是必需的，因为事件API使用内容片段列表组件，该组件不专门引用内容片段。
1.导航至AEM **[!UICONTROL >资]产[!UICONTROL > Assets]>所有文件> 1。选择内容[!UICONTROL >片段。点按出版物][!DNL WKND Mobile][!DNL English][!DNL Events]****[!DNL Event]***********************[!DNL Events]。将顶部操作栏中的默认离开PublishAction 1，默认PublishAction为-Action 1，点按前一操作栏中的下一个，SelectAlF。点按顶部操作栏中的发布内容片段模型，引用事件图像将与内容片段一起自动发布。*

1. 发布页 **[!DNL Events API]面**。
   1. 导航到 **[!UICONTROL AEM]>[!UICONTROL 站点]>[!DNL WKND Mobile]>[!DNL English]>[!DNL API]**
   1. 选择页 **[!DNL Events]** 面
   1. 点按顶 **[!UICONTROL 部操作栏]** 中的管理发布
   1. 保留默认 **的** “发布”操作原样，点 **[!UICONTROL 按顶部操]** 作栏中的“下一步”
   1. 选择页 **[!DNL Events]** 面
   1. 点 **[!DNL Publish]** 按顶部操作栏

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## 验证AEM发布

1. 在新的Web浏览器中，确保您已注销AEM发布并请求以下URL(替 `http://localhost:4503` 换正在运行的任何主机：端口AEM发布)。

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   这些请求应返回与审阅相应AEM作者端点时相同的JSON响应。 如果不这样做，请确保所有发布都成功（检查复制队列）, [!DNL WKND Mobile] 将包 `ui.apps` 安装在AEM发布上，并查看 `error.log` AEM发布。

## 下一步

无需安装额外的包。 确保将本节中概述的内容和配置发布到AEM发布，否则后续章节将不起作用。

* [第7章——使用移动应用程序中的AEM内容服务](./chapter-7.md)
