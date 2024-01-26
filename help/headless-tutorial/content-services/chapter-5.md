---
title: 第5章 — 创作Content Services页面 — Content Services
description: AEM Headless教程的第5章介绍了如何从第4章中定义的模板创建页面。 这些页面将用作JSON HTTP端点。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 873d8e69-5a05-44ac-8dae-bba21f82b823
duration: 227
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '570'
ht-degree: 0%

---

# 第5章 — 创作Content Services页面

AEM Headless教程的第5章介绍了如何从第4章中定义的模板创建页面。 本章中创建的页面将用作移动设备应用程序的JSON HTTP端点。

>[!NOTE]
>
> 的页面内容架构 `/content/wknd-mobile/en/api` 已预建。 的基础页面 `en` 和 `api` 用于架构和组织目的，但并非绝对必需。 如果API内容可能已本地化，则最佳实践是遵循常见的语言副本和多站点管理器页面组织最佳实践，因为API页面可以像任何AEM Sites页面一样进行本地化。

## 创建事件API页面

1. 导航到 **[!UICONTROL AEM] > [!UICONTROL 站点] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**.
1. **点按API页面上的标签**，然后点按 **创建** 按钮，并在API页面下创建新的Events API页面。
   1. 点按 **创建** 在顶部操作栏中
   1. 选择 **事件API** 模板
   1. 在 **名称** 字段输入 **事件**
   1. 在 **标题** 字段输入 **事件API**
   1. 点按 **创建** 创建页面的顶部操作栏
   1. 点按 **完成** 以返回AEM Sites管理员

>[!VIDEO](https://video.tv.adobe.com/v/28340?quality=12&learn=on)

## 创作事件API页面

>[!NOTE]
>
> 项目提供CSS以便为创作体验提供一些基本样式。

1. 编辑 **事件API** 页面，导航到 **AEM >站点> WKND Mobile >英语> API**，选择 **事件API** 页面，然后点按 **编辑** 在顶部操作栏中。
1. 添加 **徽标图像** 在应用程序中显示，方法是将其从资产查找器拖放到图像组件占位符上。
   * 使用提供的徽标，网址为 `/content/dam/wknd-mobile/images/wknd-logo.png`.

1. 添加 **标记行** 以显示在事件上方。
   1. 编辑 **文本** 组件
   1. 输入文本：
      * `The WKND is here.`

1. 选择 **事件** 要显示，请执行以下操作：
   1. 在上设置以下配置 **属性** 选项卡：
      * 型号： **事件**
      * 父路径： **/content/dam/wknd-mobile/en/events**
      * 标记： **&lt;leave blank=&quot;&quot;>**
   1. 在上设置以下配置 **元素** 选项卡：
      * 请删除列出的所有元素，以确保公开事件内容片段的所有元素。

>[!VIDEO](https://video.tv.adobe.com/v/28339?quality=12&learn=on)

## 查看API页面的JSON输出

可通过以下方式请求页面，查看JSON输出及其格式 `.model.json` 选择器。

此API的使用者必须充分了解此JSON结构（或架构）。 API使用者必须了解结构的哪些方面是固定的(即 活动API的徽标（图像）和实时标记（文本）是流畅的(即 在内容片段列表组件下列出的事件)。

如果违反了已发布API上的此合同，可能会导致使用应用程序中的行为不正确。

1. 在新的浏览器选项卡中，使用请求事件API页面 `.model.json` 选择器调用AEM Content Services的JSON导出器，并将页面和组件序列化为规范化、定义良好的JSON结构。

   这些页面生成的JSON结构是使用应用程序必须调整的结构。

1. 请求 **事件API** 页面为 **JSON**.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   结果应类似于：

![AEM Content Services JSON输出](assets/chapter-5/json-output.png)

>[!NOTE]
>
> 此JSON可在 **整洁** （格式化）风格的人可读性，使用 `.tidy` 选择器：
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

## 下一步

（可选）安装 [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) AEM创作中的内容包，通过 [AEM包管理器](http://localhost:4502/crx/packmgr/index.jsp). 此资源包包含本教程及前面章节中概述的配置及内容。

* [第6章 — 在AEM Publish上以JSON形式公开内容](./chapter-6.md)
