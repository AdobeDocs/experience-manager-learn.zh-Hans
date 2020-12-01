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

1. 确保使用[!UICONTROL 包管理器]在[第1章](./chapter-1.md#wknd-mobile-application-packages)中列出的&#x200B;**[!DNL WKND Mobile]应用程序包**&#x200B;安装在&#x200B;**AEM发布**&#x200B;上。
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. 发布&#x200B;**[!DNL WKND Mobile Events API]可编辑模板**
   1. 导航到&#x200B;**[!UICONTROL AEM] > [!UICONTROL 工具] > [!UICONTROL 常规] > [!UICONTROL 模板] >[!DNL WKND Mobile]**
   1. 选择&#x200B;**[!DNL Event API]**&#x200B;模板
   1. 点按顶部操作栏中的&#x200B;**[!UICONTROL 发布]**
   1. 发布&#x200B;**template**&#x200B;和&#x200B;**所有引用**（内容策略、内容策略映射和模板）

1. 发布&#x200B;**[!DNL WKND Mobile Events]内容片段**。

请注意，这是必需的，因为事件API使用内容片段列表组件，该组件不专门引用内容片段。
1.导航到**[!UICONTROL AEM] > [!UICONTROL 资产] > [!UICONTROL 文件] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
1.选择所有**[!DNL Event]**内容片段
1.点按顶部操作栏中的**[!UICONTROL 管理发布]**
1.保留默认的**“按原样发布**&#x200B;操作”，点按顶部操作栏中的&#x200B;**[!UICONTROL 下一步]**
1.选择**所有**内容片段
1.点按顶部操作栏中的**[!UICONTROL 发布]**
* *[!DNL Events]内容片段模型和引用事件图像将自动与内容片段一起发布。*

1. 发布&#x200B;**[!DNL Events API]页面**。
   1. 导航到&#x200B;**[!UICONTROL AEM] > [!UICONTROL 站点] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. 选择&#x200B;**[!DNL Events]**&#x200B;页面
   1. 点按顶部操作栏中的&#x200B;**[!UICONTROL 管理发布]**
   1. 将默认的&#x200B;**Publish**&#x200B;操作原样保留，点按顶部操作栏中的&#x200B;**[!UICONTROL Next]**
   1. 选择&#x200B;**[!DNL Events]**&#x200B;页面
   1. 点按顶部操作栏中的&#x200B;**[!DNL Publish]**

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## 验证AEM发布

1. 在新的Web浏览器中，确保您已注销AEM发布并请求以下URL（用`http://localhost:4503`替换运行AEM发布的任何主机：端口）。

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   这些请求应返回与审阅相应AEM作者端点时相同的JSON响应。 如果不这样做，请确保所有发布都成功（检查复制队列）,[!DNL WKND Mobile] `ui.apps`包安装在AEM发布上，并查看`error.log`的AEM发布。

## 下一步

无需安装额外的包。 确保将本节中概述的内容和配置发布到AEM发布，否则后续章节将不起作用。

* [第7章——使用移动应用程序中的AEM内容服务](./chapter-7.md)
