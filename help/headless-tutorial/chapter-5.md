---
title: 第5章——创作内容服务页面
description: AEM无头教程的第5章介绍了如何根据第4章中定义的模板创建页面。 这些页面将充当JSON HTTP端点。
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '594'
ht-degree: 0%

---


# 第5章——创作内容服务页面

AEM无头教程的第5章介绍了如何根据第4章中定义的模板创建页面。 本章中创建的页面将充当移动应用程序的JSON HTTP端点。

>[!NOTE]
>
> 的页面内容 `/content/wknd-mobile/en/api` 架构已预建。 基本页面 `en` 和服 `api` 务于建筑和组织目的，但并不严格要求。 如果API内容可能已本地化，则最好遵循通常的“语言副本”和“多站点管理器”页面组织最佳实践，因为API页面可以像AEM Sites页面中的任何页面一样本地化。

## 创建事件API页

1. 导航到 **[!UICONTROL AEM] > [!UICONTROL 站点] > [!DNL WKND Mobile] > [!DNL English][!DNL API]**> 。
1. **点按API页面的标签**，然后点按顶 **部操作** 栏中的“创建”按钮，并在API页面下创建新的事件API页面。
   1. 点 **按顶部** 操作栏中的创建
   1. 选择 **事件API** 模板
   1. 在“名 **称** ”字段中输 **入事件**
   1. 在“标 **题** ”字段中输 **入事件API**
   1. 点 **按顶部** 操作栏中的创建以创建页面
   1. 点按 **完成** ，返回AEM Sites管理员

>[!VIDEO](https://video.tv.adobe.com/v/28340/?quality=12&learn=on)

## 创作事件API页

>[!NOTE]
>
> 项目提供CSS，以便为创作体验提供一些基本样式。

1. 编辑 **事件** API页面，方法是导航到 **AEM >站点> WKND Mobile > English > API**，选择 **事件API页面，并点** 击顶级操作栏 **** 中的编辑。
1. 通过将 **徽标图像** 从资产查找器拖放到图像组件占位符上，添加要在应用程序中显示的图像。
   * 使用上提供的徽标 `/content/dam/wknd-mobile/images/wknd-logo.png`。

1. 添 **加标签行** ，以在事件上方显示。
   1. 编辑文 **本组** 件
   1. 输入文本：
      * `The WKND is here.`

1. 选择要 **显示的** 事件:
   1. 在“属性”选项卡上设 **置以下** 配置：
      * 模型： **事件**
      * 父路径： **/content/dam/wknd-mobile/cn/事件**
      * 标记： **&lt;留空>**
   1. 在“元素”选项卡上设置 **以下** 配置：
      * 删除所有列出的元素，确保事件内容片段的所有元素都公开。

>[!VIDEO](https://video.tv.adobe.com/v/28339/?quality=12&learn=on)

## 查看API页面的JSON输出

可通过使用选择器请求页面来查看JSON输出及其格 `.model.json` 式。

此API的消费者必须充分理解此JSON结构(或模式)。 关键的API用户了解结构的哪些方面是固定的(即， 事件API的徽标（图像）和实时标记（文本），它们是流畅的(即 内容片段事件组件下列出的列表)。

在已发布的API上违反本合同，可能会导致在使用中的应用程序中出现错误行为。

1. 在新的浏览器选项卡中，使用选择器请求事件API页 `.model.json` 面，该选择器调用AEM Content Services的JSON导出器，并将页面和组件序列化为标准化、定义良好的JSON结构。

   这些页面生成的JSON结构是使用应用程序必须与之对齐的结构。

1. 将事件 **API页** 作为JSON **请求**。

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   结果应类似于：

![AEM Content Services JSON输出](assets/chapter-5/json-output.png)

>[!NOTE]
>
> 此JSON可以采用整洁(格 **式** )的方式输出，以便于人的可 `.tidy` 读性：
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## 下一步

（可选）通 [过AEM包管理器在AEM作者上安装com.adobe.aem.guides.wknd-mobile.content.chapter](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) -5.zip [内容包](http://localhost:4502/crx/packmgr/index.jsp)。 此包包含教程本章和前几章中概述的配置和内容。

* [第6章——将AEM发布中的内容公开为JSON](./chapter-6.md)
