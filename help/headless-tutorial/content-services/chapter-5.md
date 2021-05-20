---
title: 第5章 — 创作内容服务页面 — 内容服务
description: AEM Headless教程的第5章介绍了如何使用第4章中定义的模板创建页面。 这些页面将用作JSON HTTP端点。
feature: 内容片段、 API
topic: 无外设、内容管理
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '602'
ht-degree: 0%

---


# 第5章 — 创作内容服务页面

《AEM Headless》教程的第5章介绍了如何通过第4章中定义的模板创建页面。 本章中创建的页面将充当移动设备应用程序的JSON HTTP端点。

>[!NOTE]
>
> 已预建`/content/wknd-mobile/en/api`的页面内容架构。 `en`和`api`的基页具有体系结构和组织用途，但并非严格要求。 如果API内容可能会进行本地化，最佳做法是遵循常规的语言副本和多站点管理器页面组织最佳实践，因为API页面可以像任何AEM Sites页面一样进行本地化。

## 创建事件API页面

1. 导航到&#x200B;**[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**。
1. **点按API页面的标签**，然后点按顶部操 **** 作栏中的创建按钮，并在API页面下创建新的事件API页面。
   1. 点按顶部操作栏中的&#x200B;**创建**
   1. 选择&#x200B;**事件API**&#x200B;模板
   1. 在&#x200B;**Name**&#x200B;字段中，输入&#x200B;**events**
   1. 在&#x200B;**标题**&#x200B;字段中，输入&#x200B;**事件API**
   1. 点按顶部操作栏中的&#x200B;**创建**&#x200B;以创建页面
   1. 点按&#x200B;**完成**&#x200B;以返回AEM Sites管理员

>[!VIDEO](https://video.tv.adobe.com/v/28340/?quality=12&learn=on)

## “创作事件API”页面

>[!NOTE]
>
> 项目提供了CSS，以便为创作体验提供一些基本样式。

1. 编辑&#x200B;**事件API**&#x200B;页面，方法是导航到&#x200B;**AEM >站点> WKND移动设备>英语> API**，选择&#x200B;**事件API**&#x200B;页面，然后点按顶部操作栏中的&#x200B;**编辑**。
1. 通过将&#x200B;**徽标图像**&#x200B;从资产查找器中拖放到图像组件占位符，可添加要在应用程序中显示的徽标图像。
   * 使用`/content/dam/wknd-mobile/images/wknd-logo.png`中提供的徽标。

1. 添加&#x200B;**标记行**&#x200B;以在事件上方显示。
   1. 编辑&#x200B;**Text**&#x200B;组件
   1. 输入文本：
      * `The WKND is here.`

1. 选择要显示的&#x200B;**事件**:
   1. 在&#x200B;**Properties**&#x200B;选项卡中设置以下配置：
      * 模型：**事件**
      * 父路径：**/content/dam/wknd-mobile/en/events**
      * 标记：**&lt;留空>**
   1. 在&#x200B;**Elements**&#x200B;选项卡中设置以下配置：
      * 请移除所有列出的元素，以确保事件内容片段的所有元素都已公开。

>[!VIDEO](https://video.tv.adobe.com/v/28339/?quality=12&learn=on)

## 查看API页面的JSON输出

通过使用`.model.json`选择器请求页面，可以查看JSON输出及其格式。

此API的用户必须熟悉此JSON结构（或架构）。 关键的API用户要了解结构的哪些方面是固定的(即， 事件API的徽标（图像）和标记实时（文本），它们是不固定的(即 内容片段列表组件下列出的事件)。

如果在已发布的API上违反此合同，则可能会导致使用的应用程序中的行为不正确。

1. 在新的浏览器选项卡中，使用`.model.json`选择器请求事件API页面，该选择器会调用AEM Content Services的JSON导出程序，并将页面和组件序列化为标准化且定义良好的JSON结构。

   这些页面生成的JSON结构是使用应用程序必须与之保持一致的结构。

1. 请求将&#x200B;**事件API**&#x200B;页面作为&#x200B;**JSON**。

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   结果应类似于：

![AEM Content Services JSON输出](assets/chapter-5/json-output.png)

>[!NOTE]
>
> 使用`.tidy`选择器，可以以&#x200B;**tidy**（格式化）方式输出此JSON，以方便人们阅读：
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## 下一步

或者，也可以选择通过[AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)在AEM作者上安装[com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)内容包。 此包包含教程本章及前几章中概述的配置和内容。

* [第6章 — 在AEM发布中以JSON形式公开内容](./chapter-6.md)
