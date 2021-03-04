---
title: 第5章 — 创作内容服务页面 — 内容服务
description: AEM无标题教程的第5章介绍了如何从第4章中定义的模板创建页面。 这些页面将用作JSON HTTP端点。
feature: 内容片段、API
topic: 无头、内容管理
role: 开发人员
level: 初学者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '604'
ht-degree: 0%

---


# 第5章 — 创作内容服务页面

AEM无头教程的第5章介绍了如何从第4章中定义的模板创建页面。 本章中创建的页面将用作移动应用程序的JSON HTTP端点。

>[!NOTE]
>
> `/content/wknd-mobile/en/api`的页面内容架构已预建。 `en`和`api`的基页用于架构和组织目的，但并不严格要求。 如果API内容可能已本地化，则最好遵循通常的“语言副本”和“多站点管理器”页面组织最佳实践，因为API页面可以像任何AEM Sites页面一样本地化。

## 创建事件API页

1. 导航到&#x200B;**[!UICONTROL AEM] > [!UICONTROL 站点] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**。
1. **点按API页面的标签**，然后点 **** 按顶部操作栏中的“创建”按钮，并在API页面下创建新的事件API页面。
   1. 点按顶部操作栏中的&#x200B;**创建**
   1. 选择&#x200B;**事件API**&#x200B;模板
   1. 在&#x200B;**名称**&#x200B;字段中，输入&#x200B;**事件**
   1. 在&#x200B;**标题**&#x200B;字段中，输入&#x200B;**事件API**
   1. 点按顶部操作栏中的&#x200B;**创建**&#x200B;以创建页面
   1. 点按&#x200B;**完成**&#x200B;以返回AEM Sites管理员

>[!VIDEO](https://video.tv.adobe.com/v/28340/?quality=12&learn=on)

## 创作事件API页

>[!NOTE]
>
> 项目提供CSS以便为创作体验提供一些基本样式。

1. 通过导航到&#x200B;**AEM >站点> WKND Mobile >英语> API**，选择&#x200B;**事件API**&#x200B;页面，然后点按顶部操作栏中的&#x200B;**编辑**，编辑&#x200B;**事件API**&#x200B;页面。
1. 通过将&#x200B;**徽标图像**&#x200B;从资产查找器拖放到图像组件占位符上，添加要在应用程序中显示的徽标图像。
   * 使用`/content/dam/wknd-mobile/images/wknd-logo.png`中提供的徽标。

1. 添加&#x200B;**标记行**&#x200B;以在事件上方显示。
   1. 编辑&#x200B;**Text**&#x200B;组件
   1. 输入文本：
      * `The WKND is here.`

1. 选择要显示的&#x200B;**事件**:
   1. 在&#x200B;**属性**&#x200B;选项卡上设置以下配置：
      * 模型：**事件**
      * 父路径：**/content/dam/wknd-mobile/cn/事件s**
      * 标记：**&lt;留空>**
   1. 在&#x200B;**Elements**&#x200B;选项卡上设置以下配置：
      * 删除所有列出的元素，以确保事件内容片段的所有元素都公开。

>[!VIDEO](https://video.tv.adobe.com/v/28339/?quality=12&learn=on)

## 查看API页的JSON输出

可通过使用`.model.json`选择器请求页面来查看JSON输出及其格式。

此API的消费者必须充分理解此JSON结构(或模式)。 关键的API用户了解结构的哪些方面是固定的(即 事件API的徽标（图像）和实时标记（文本），它们是流畅的(即 内容片段事件组件下列出的列表)。

在已发布的API上违反本合同，可能会导致在使用中的应用程序中出现错误行为。

1. 在新的浏览器选项卡中，使用`.model.json`选择器请求事件API页面，该选择器调用AEM Content Services的JSON导出器，并将页面和组件序列化为标准化的、定义良好的JSON结构。

   这些页面生成的JSON结构是使用应用程序必须与之对齐的结构。

1. 请求&#x200B;**事件API**&#x200B;页作为&#x200B;**JSON**。

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   结果应类似于：

![AEM Content Services JSON输出](assets/chapter-5/json-output.png)

>[!NOTE]
>
> 使用`.tidy`选择器，可以以&#x200B;**tidy**（格式化）方式输出此JSON，以便于人的可读性：
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## 下一步

或者，通过[AEM包管理器](http://localhost:4502/crx/packmgr/index.jsp)在AEM作者上安装[com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)内容包。 此包包含教程本章和前几章中概述的配置和内容。

* [第6章 — 将AEM发布上的内容公开为JSON](./chapter-6.md)
