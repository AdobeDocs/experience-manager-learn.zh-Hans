---
title: 第6章 — 将AEM发布中的内容公开为JSON — 内容服务
description: AEM Headless教程的第6章介绍了如何确保在AEM发布上存在所有必需的包、配置和内容，以便允许从移动设备应用程序中使用这些内容。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: b33d1509-531d-40c3-9b26-1d18c8d86a97
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 0%

---

# 第6章 — 在AEM发布中公开内容以进行交付

AEM Headless教程的第6章介绍了如何确保在AEM发布上安装所有必需的包、配置和内容，以便移动设备应用程序能够使用这些内容。

## 发布AEM Content Services的内容

为通过AEM Content Services驱动事件而创建的配置和内容必须发布到AEM发布，以便移动设备应用程序可以访问该发布。

由于AEM内容服务是通过配置（内容片段模型、可编辑模板）、资产（内容片段、图像）和页面构建的，因此所有这些部分都会自动享受AEM内容管理功能，包括：

* 用于审核和处理的工作流
* 用于从AEM发布的AEM Content Services端点推送和提取内容的激活/停用

1. 确保 **[!DNL WKND Mobile]应用程序包**，在 [第1章](./chapter-1.md#wknd-mobile-application-packages)，则安装在 **AEM发布** 使用 [!UICONTROL 包管理器].
   * [http://localhost:4503/crx/packmgr](http://localhost:4503/crx/packmgr)

1. 发布 **[!DNL WKND Mobile Events API]可编辑的模板**
   1. 导航到 **[!UICONTROL AEM] > [!UICONTROL 工具] > [!UICONTROL 常规] > [!UICONTROL 模板] >[!DNL WKND Mobile]**
   1. 选择 **[!DNL Event API]** 模板
   1. 点按 **[!UICONTROL 发布]** 在顶部操作栏中
   1. 发布 **模板** 和 **所有引用** （内容策略、内容策略映射和模板）

1. 发布 **[!DNL WKND Mobile Events]内容片段**.

   请注意，这是必需的，因为事件API使用内容片段列表组件，该组件不专门引用内容片段。

   1. 导航到 **[!UICONTROL AEM] > [!UICONTROL 资产] > [!UICONTROL 文件] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**
   1. 选择所有 **[!DNL Event]** 内容片段
   1. 点按 **[!UICONTROL 管理发布]** 在顶部操作栏中
   1. 保留默认 **发布** 按原样执行操作，点按 **[!UICONTROL 下一个]** 在顶部操作栏中
   1. 选择 **全部** 内容片段
   1. 点按 **[!UICONTROL 发布]** 在顶部操作栏中
      * *的 [!DNL Events] 内容片段模型和引用事件图像将与内容片段一起自动发布。*

1. 发布 **[!DNL Events API]页面**.
   1. 导航到 **[!UICONTROL AEM] > [!UICONTROL 站点] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**
   1. 选择 **[!DNL Events]** 页面
   1. 点按 **[!UICONTROL 管理发布]** 在顶部操作栏中
   1. 保留默认 **发布** 按原样执行操作，点按 **[!UICONTROL 下一个]** 在顶部操作栏中
   1. 选择 **[!DNL Events]** 页面
   1. 点按 **[!DNL Publish]** 在顶部操作栏中

>[!VIDEO](https://video.tv.adobe.com/v/28343?quality=12&learn=on)

## 验证AEM发布

1. 在新的Web浏览器中，确保您已注销AEM发布，并请求以下URL(替换 `http://localhost:4503` 对于任何主机：AEM发布运行的端口)。

   * [http://localhost:4503/content/wknd-mobile/en/api/events.model.json](http://localhost:4503/content/wknd-mobile/en/api/events.model.tidy.json)
   这些请求应返回与审核相应AEM作者端点时相同的JSON响应。 如果没有，请确保所有发布都成功（检查复制队列）， [!DNL WKND Mobile] `ui.apps` 包安装在AEM发布上，并查看 `error.log` （针对AEM发布）。

## 下一步

没有要安装的额外包。 确保将此部分中概述的内容和配置发布到AEM发布，否则后续章节将不起作用。

* [第7章 — 从移动设备应用程序使用AEM内容服务](./chapter-7.md)
