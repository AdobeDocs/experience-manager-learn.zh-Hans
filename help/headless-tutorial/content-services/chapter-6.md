---
title: 第6章 — 在AEM Publish上以JSON形式公开内容 — Content Services
description: AEM Headless教程的第6章涵盖确保所有必要的包、配置和内容都位于AEM Publish上，以允许从移动应用程序使用。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: b33d1509-531d-40c3-9b26-1d18c8d86a97
duration: 196
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 0%

---

# 第6章 — 在AEM Publish上公开内容以进行交付

AEM Headless教程的第6章涵盖确保所有必要的包、配置和内容都位于AEM Publish上，以允许移动设备应用程序使用。

## 发布AEM内容服务的内容

为通过AEM Content Services驱动事件而创建的配置和内容必须发布到AEM Publish，以便移动设备应用程序可以访问该配置。

由于AEM Content Services是根据配置（内容片段模型、可编辑模板）、Assets（内容片段、图像）和页面构建的，因此所有这些部分都会自动享受AEM的内容管理功能，包括：

* 用于审阅和处理的工作流
* 和用于从AEM Publish的AEM Content Services端点推送和提取内容的激活/停用

1. 确保使用[!UICONTROL 包管理器]在&#x200B;**AEM Publish**&#x200B;上安装了[第1](./chapter-1.md#wknd-mobile-application-packages)章中列出的&#x200B;**[!DNL WKND Mobile]应用程序包**。
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. Publish **[!DNL WKND Mobile Events API]可编辑模板**
   1. 导航到&#x200B;**[!UICONTROL AEM] > [!UICONTROL 工具] > [!UICONTROL 常规] > [!UICONTROL 模板] >[!DNL WKND Mobile]**
   1. 选择&#x200B;**[!DNL Event API]**&#x200B;模板
   1. 点按顶部操作栏中的&#x200B;**[!UICONTROL Publish]**
   1. Publish **模板**&#x200B;和&#x200B;**所有引用**（内容策略、内容策略映射和模板）

1. Publish **[!DNL WKND Mobile Events]内容片段**。

   请注意，这是必需的，因为事件API使用内容片段列表组件，该组件未专门引用内容片段。

   1. 导航到&#x200B;**[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL 文件] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
   1. 选择所有&#x200B;**[!DNL Event]**&#x200B;内容片段
   1. 点按顶部操作栏中的&#x200B;**[!UICONTROL 管理发布]**
   1. 将默认&#x200B;**Publish**&#x200B;操作保持原样，点按顶部操作栏中的&#x200B;**[!UICONTROL 下一步]**
   1. 选择&#x200B;**所有**&#x200B;内容片段
   1. 点按顶部操作栏中的&#x200B;**[!UICONTROL Publish]**
      * *将自动发布[!DNL Events]内容片段模型和引用事件图像以及内容片段。*

1. Publish **[!DNL Events API]页面**。
   1. 导航到&#x200B;**[!UICONTROL AEM] > [!UICONTROL 站点] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. 选择&#x200B;**[!DNL Events]**&#x200B;页面
   1. 点按顶部操作栏中的&#x200B;**[!UICONTROL 管理发布]**
   1. 将默认&#x200B;**Publish**&#x200B;操作保持原样，点按顶部操作栏中的&#x200B;**[!UICONTROL 下一步]**
   1. 选择&#x200B;**[!DNL Events]**&#x200B;页面
   1. 点按顶部操作栏中的&#x200B;**[!DNL Publish]**

>[!VIDEO](https://video.tv.adobe.com/v/28343?quality=12&learn=on)

## 验证AEM Publish

1. 在新的Web浏览器中，确保您已从AEM Publish中注销，并请求以下URL(将`http://localhost:4503`替换为AEM Publish正在运行的任何主机：端口)。

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)

   这些请求应返回与审查相应的AEM创作端点时相同的JSON响应。 如果不成功，请确保所有发布都成功（检查复制队列），[!DNL WKND Mobile] `ui.apps`包安装在AEM Publish上，并查看AEM Publish的`error.log`。

## 下一步

没有要安装的额外包。 请确保将此部分中概述的内容和配置发布到AEM Publish，否则后续章节将无法正常工作。

* [第7章 — 从移动设备应用程序使用AEM Content Services](./chapter-7.md)
