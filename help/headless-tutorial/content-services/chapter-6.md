---
title: 第6章 — 在AEM Publish上以JSON形式公开内容 — 内容服务
description: AEM Headless教程的第6章涵盖确保所有必要的包、配置和内容都位于AEM Publish上，以允许从移动设备应用程序使用。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: b33d1509-531d-40c3-9b26-1d18c8d86a97
duration: 210
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 0%

---

# 第6章 — 在AEM Publish上公开内容以进行交付

AEM Headless教程的第6章涵盖确保所有必要的包、配置和内容都位于AEM Publish上，以允许移动设备应用程序使用。

## 发布AEM内容服务的内容

为通过AEM Content Services驱动事件而创建的配置和内容必须发布到AEM Publish，以便移动设备应用程序可以访问该配置。

由于AEM Content Services是根据配置（内容片段模型、可编辑模板）、资产（内容片段、图像）和页面构建的，因此所有这些部分都会自动享受AEM内容管理功能，包括：

* 用于审阅和处理的工作流
* 和激活/停用从AEM Publish的AEM Content Services端点推送和提取内容

1. 确保 **[!DNL WKND Mobile]应用程序包**，在中列出 [第1章](./chapter-1.md#wknd-mobile-application-packages)，安装在 **AEM发布** 使用 [!UICONTROL 包管理器].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. 发布 **[!DNL WKND Mobile Events API]可编辑的模板**
   1. 导航到 **[!UICONTROL AEM] > [!UICONTROL 工具] > [!UICONTROL 常规] > [!UICONTROL 模板] >[!DNL WKND Mobile]**
   1. 选择 **[!DNL Event API]** 模板
   1. 点按 **[!UICONTROL Publish]** 在顶部操作栏中
   1. 发布 **模板** 和 **所有引用** （内容策略、内容策略映射和模板）

1. 发布 **[!DNL WKND Mobile Events]内容片段**.

   请注意，这是必需的，因为事件API使用内容片段列表组件，该组件未专门引用内容片段。

   1. 导航到 **[!UICONTROL AEM] > [!UICONTROL 资产] > [!UICONTROL 文件] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
   1. 选择所有 **[!DNL Event]** 内容片段
   1. 点按 **[!UICONTROL 管理发布]** 在顶部操作栏中
   1. 保留默认值 **Publish** 按原样操作，点按 **[!UICONTROL 下一个]** 在顶部操作栏中
   1. 选择 **所有** 内容片段
   1. 点按 **[!UICONTROL Publish]** 在顶部操作栏中
      * *此 [!DNL Events] 内容片段模型和引用事件图像将自动与内容片段一起发布。*

1. 发布 **[!DNL Events API]页面**.
   1. 导航到 **[!UICONTROL AEM] > [!UICONTROL 站点] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. 选择 **[!DNL Events]** 页面
   1. 点按 **[!UICONTROL 管理发布]** 在顶部操作栏中
   1. 保留默认值 **Publish** 按原样操作，点按 **[!UICONTROL 下一个]** 在顶部操作栏中
   1. 选择 **[!DNL Events]** 页面
   1. 点按 **[!DNL Publish]** 在顶部操作栏中

>[!VIDEO](https://video.tv.adobe.com/v/28343?quality=12&learn=on)

## 验证AEM发布

1. 在新的Web浏览器中，确保您已注销AEM Publish并请求以下URL(替换 `http://localhost:4503` 适用于运行AEM Publish的任何主机：端口)。

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)

   这些请求应返回与审查相应的AEM创作端点时相同的JSON响应。 如果不成功，请确保所有发布都成功（检查复制队列）， [!DNL WKND Mobile] `ui.apps` 包安装在AEM Publish上，并查看 `error.log` 用于AEM发布。

## 下一步

没有要安装的额外包。 请确保将此部分中概述的内容和配置发布到AEM Publish，否则后续章节将无法正常使用。

* [第7章 — 从移动设备应用程序使用AEM Content Services](./chapter-7.md)
