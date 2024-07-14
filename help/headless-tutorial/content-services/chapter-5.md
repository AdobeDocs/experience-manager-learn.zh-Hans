---
title: 第5章 — 创作Content Services页面 — Content Services
description: AEM Headless教程的第5章介绍了如何从第4章中定义的模板创建页面。 这些页面将用作JSON HTTP端点。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 873d8e69-5a05-44ac-8dae-bba21f82b823
duration: 189
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '570'
ht-degree: 0%

---

# 第5章 — 创作Content Services页面

AEM Headless教程的第5章介绍了如何从第4章中定义的模板创建页面。 本章中创建的页面将用作移动设备应用程序的JSON HTTP端点。

>[!NOTE]
>
> 已预建`/content/wknd-mobile/en/api`的页面内容架构。 `en`和`api`的基页用于架构和组织目的，但不是严格必需的。 如果API内容可能已本地化，则最佳实践是遵循常见的语言副本和多站点管理器页面组织最佳实践，因为API页面可以像任何AEM Sites页面一样进行本地化。

## 创建事件API页面

1. 导航到&#x200B;**[!UICONTROL AEM] > [!UICONTROL 站点] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**。
1. **点按API页面标签**，然后点按顶部操作栏中的&#x200B;**创建**&#x200B;按钮，并在API页面下创建新的Events API页面。
   1. 点按顶部操作栏中的&#x200B;**创建**
   1. 选择&#x200B;**事件API**&#x200B;模板
   1. 在&#x200B;**名称**&#x200B;字段中，输入&#x200B;**事件**
   1. 在&#x200B;**标题**&#x200B;字段中，输入&#x200B;**事件API**
   1. 点按顶部操作栏中的&#x200B;**创建**&#x200B;以创建页面
   1. 点按&#x200B;**完成**&#x200B;以返回到AEM Sites管理员

>[!VIDEO](https://video.tv.adobe.com/v/28340?quality=12&learn=on)

## 创作事件API页面

>[!NOTE]
>
> 项目提供CSS以便为创作体验提供一些基本样式。

1. 通过导航到&#x200B;**AEM > Sites > WKND Mobile >英语> API**，选择&#x200B;**Events API**&#x200B;页面，并点按顶部操作栏中的&#x200B;**编辑**，编辑&#x200B;**Events API**&#x200B;页面。
1. 添加要在应用程序中显示的&#x200B;**徽标图像**，方法是将其从资产查找器拖放到图像组件占位符上。
   * 使用在`/content/dam/wknd-mobile/images/wknd-logo.png`找到的提供的徽标。

1. 添加要在事件上方显示的&#x200B;**标记行**。
   1. 编辑&#x200B;**Text**&#x200B;组件
   1. 输入文本：
      * `The WKND is here.`

1. 选择要显示的&#x200B;**事件**：
   1. 在&#x200B;**属性**&#x200B;选项卡上设置以下配置：
      * 模型： **事件**
      * 父路径： **/content/dam/wknd-mobile/en/events**
      * 标记： **&lt;留空>**
   1. 在&#x200B;**元素**&#x200B;选项卡上设置以下配置：
      * 请删除列出的所有元素，以确保公开事件内容片段的所有元素。

>[!VIDEO](https://video.tv.adobe.com/v/28339?quality=12&learn=on)

## 查看API页面的JSON输出

可以通过请求包含`.model.json`选择器的页面来查看JSON输出及其格式。

此API的使用者必须充分了解此JSON结构（或架构）。 API使用者必须了解结构的哪些方面是固定的(即 活动API的徽标（图像）和实时标记（文本）是流畅的(即 在内容片段列表组件下列出的事件)。

如果违反了已发布API上的此合同，可能会导致使用应用程序中的行为不正确。

1. 在新的浏览器选项卡中，使用`.model.json`选择器请求Events API页面，该选择器会调用AEM Content Services的JSON导出器，并将页面和组件序列化为规范化、定义良好的JSON结构。

   这些页面生成的JSON结构是使用应用程序必须调整的结构。

1. 请求&#x200B;**事件API**&#x200B;页面作为&#x200B;**JSON**。

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   结果应类似于：

![AEM Content Services JSON输出](assets/chapter-5/json-output.png)

>[!NOTE]
>
> 可以使用`.tidy`选择器以&#x200B;**整齐**（格式化）方式输出此JSON，以便于用户阅读：
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

## 下一步

或者，通过[AEM包管理器](http://localhost:4502/crx/packmgr/index.jsp)在AEM Author上安装[com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)内容包。 此资源包包含本教程及前面章节中概述的配置及内容。

* [第6章 — 在AEM Publish上以JSON形式公开内容](./chapter-6.md)
