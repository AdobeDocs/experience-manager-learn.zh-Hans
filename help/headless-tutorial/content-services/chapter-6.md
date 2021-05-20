---
title: 第6章 — 将AEM发布中的内容公开为JSON — 内容服务
description: AEM Headless教程的第6章介绍了如何确保在AEM发布上存在所有必需的包、配置和内容，以便允许从移动设备应用程序中使用这些内容。
feature: 内容片段、 API
topic: 无外设、内容管理
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 0%

---


# 第6章 — 在AEM发布中公开内容以进行交付

AEM Headless教程的第6章介绍了如何确保在AEM发布上安装所有必需的包、配置和内容，以便移动设备应用程序能够使用这些内容。

## 发布AEM Content Services的内容

为通过AEM Content Services驱动事件而创建的配置和内容必须发布到AEM发布，以便移动设备应用程序可以访问该发布。

由于AEM内容服务是通过配置（内容片段模型、可编辑模板）、资产（内容片段、图像）和页面构建的，因此所有这些部分都会自动享受AEM内容管理功能，包括：

* 用于审核和处理的工作流
* 用于从AEM发布的AEM Content Services端点推送和提取内容的激活/停用

1. 确保在[第1章](./chapter-1.md#wknd-mobile-application-packages)中列出的&#x200B;**[!DNL WKND Mobile]应用程序包**&#x200B;使用[!UICONTROL 包管理器]安装在&#x200B;**AEM发布**&#x200B;上。
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. 发布&#x200B;**[!DNL WKND Mobile Events API]可编辑的模板**
   1. 导航到&#x200B;**[!UICONTROL AEM] > [!UICONTROL 工具] > [!UICONTROL 常规] > [!UICONTROL 模板] >[!DNL WKND Mobile]**
   1. 选择&#x200B;**[!DNL Event API]**&#x200B;模板
   1. 点按顶部操作栏中的&#x200B;**[!UICONTROL Publish]**
   1. 发布&#x200B;**模板**&#x200B;和&#x200B;**所有引用**（内容策略、内容策略映射和模板）

1. 发布&#x200B;**[!DNL WKND Mobile Events]内容片段**。

请注意，这是必需的，因为事件API使用内容片段列表组件，该组件不专门引用内容片段。
1.导航至**[!UICONTROL AEM] > [!UICONTROL 资产] > [!UICONTROL 文件] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
1.选择所有**[!DNL Event]**内容片段
1.点按顶部操作栏中的**[!UICONTROL 管理发布]**
1.将默认的**Publish**&#x200B;操作保持原样，点按顶部操作栏中的&#x200B;**[!UICONTROL Next]**
1.选择**所有**内容片段
1.点按顶部操作栏中的**[!UICONTROL Publish]**
* *[!DNL Events]内容片段模型和引用事件图像将自动与内容片段一起发布。*

1. 发布&#x200B;**[!DNL Events API]页面**。
   1. 导航到&#x200B;**[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. 选择&#x200B;**[!DNL Events]**&#x200B;页面
   1. 点按顶部操作栏中的&#x200B;**[!UICONTROL 管理发布]**
   1. 将默认的&#x200B;**Publish**&#x200B;操作保留为原样，点按顶部操作栏中的&#x200B;**[!UICONTROL Next]**
   1. 选择&#x200B;**[!DNL Events]**&#x200B;页面
   1. 点按顶部操作栏中的&#x200B;**[!DNL Publish]**

>[!VIDEO](https://video.tv.adobe.com/v/28343/?quality=12&learn=on)

## 验证AEM发布

1. 在新的Web浏览器中，确保您已注销AEM发布，并请求以下URL（用`http://localhost:4503`替换运行的任何主机：端口AEM发布）。

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   这些请求应返回与审核相应AEM作者端点时相同的JSON响应。 如果没有，请确保所有发布都成功（检查复制队列），在AEM发布上安装了[!DNL WKND Mobile] `ui.apps`包，并查看`error.log`的AEM发布信息。

## 下一步

没有要安装的额外包。 确保将此部分中概述的内容和配置发布到AEM发布，否则后续章节将不起作用。

* [第7章 — 从移动设备应用程序使用AEM内容服务](./chapter-7.md)
