---
title: 第5章 — 创作内容服务页面 — 内容服务
description: AEM Headless教程的第5章介绍了如何使用第4章中定义的模板创建页面。 这些页面将用作JSON HTTP端点。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 873d8e69-5a05-44ac-8dae-bba21f82b823
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 0%

---

# 第5章 — 创作内容服务页面

《AEM Headless 》教程的第5章介绍了如何使用第4章中定义的模板创建页面。 本章中创建的页面将充当移动设备应用程序的JSON HTTP端点。

>[!NOTE]
>
> 的页面内容架构 `/content/wknd-mobile/en/api` 已预建。 的基页 `en` 和 `api` 用于体系结构和组织目的，但并非严格要求。 如果API内容可能会进行本地化，最佳做法是遵循常规的语言副本和多站点管理器页面组织最佳实践，因为API页面可以像任何AEM Sites页面一样进行本地化。

## 创建事件API页面

1. 导航到 **[!UICONTROL AEM] > [!UICONTROL 站点] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**.
1. **点按API页面的标签**，然后点按 **创建** 按钮，并在API页面下创建新的事件API页面。
   1. 点按 **创建** 在顶部操作栏中
   1. 选择 **事件API** 模板
   1. 在 **名称** 字段输入 **事件**
   1. 在 **标题** 字段输入 **事件API**
   1. 点按 **创建** 在顶部操作栏中创建页面
   1. 点按 **完成** 返回AEM Sites管理员

>[!VIDEO](https://video.tv.adobe.com/v/28340?quality=12&learn=on)

## “创作事件API”页面

>[!NOTE]
>
> 项目提供了CSS，以便为创作体验提供一些基本样式。

1. 编辑 **事件API** 页面时，导航到 **AEM > Sites > WKND Mobile >英语> API**，选择 **事件API** 页面，然后点按 **编辑** 中。
1. 添加 **徽标图像** 从资产查找器将其拖放到图像组件占位符，以便在应用程序中显示。
   * 使用提供的徽标，该徽标位于 `/content/dam/wknd-mobile/images/wknd-logo.png`.

1. 添加 **标记行** 以显示在事件上方。
   1. 编辑 **文本** 组件
   1. 输入文本：
      * `The WKND is here.`

1. 选择 **事件** 要显示：
   1. 在 **属性** 选项卡：
      * 模型： **事件**
      * 父路径： **/content/dam/wknd-mobile/en/events**
      * 标记： **&lt;leave blank=&quot;&quot;>**
   1. 在 **元素** 选项卡：
      * 请移除所有列出的元素，以确保事件内容片段的所有元素都已公开。

>[!VIDEO](https://video.tv.adobe.com/v/28339?quality=12&learn=on)

## 查看API页面的JSON输出

请求包含的页面，可以查看JSON输出及其格式 `.model.json` 选择器。

此API的用户必须熟悉此JSON结构（或架构）。 关键的API用户要了解结构的哪些方面是固定的(即， 事件API的徽标（图像）和标记实时（文本），它们是不固定的(即 内容片段列表组件下列出的事件)。

如果在已发布的API上违反此合同，则可能会导致使用的应用程序中的行为不正确。

1. 在新的浏览器选项卡中，使用 `.model.json` 选择器，它会调用AEM内容服务的JSON导出程序，并将页面和组件序列化为标准化且定义良好的JSON结构。

   这些页面生成的JSON结构是使用应用程序必须与之保持一致的结构。

1. 请求 **事件API** 页面 **JSON**.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   结果应类似于：

![AEM Content Services JSON输出](assets/chapter-5/json-output.png)

>[!NOTE]
>
> 此JSON可以在 **整洁** （格式化）方式，以便使用 `.tidy` 选择器：
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## 下一步

（可选）安装 [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) AEM作者上的内容包(通过 [AEM包管理器](http://localhost:4502/crx/packmgr/index.jsp). 此包包含教程本章及前几章中概述的配置和内容。

* [第6章 — 在AEM发布中以JSON形式公开内容](./chapter-6.md)
