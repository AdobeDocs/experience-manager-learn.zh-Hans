---
title: 第4章——定义内容服务模板
seo-title: AEM无头入门——第4章——定义内容服务模板
description: AEM无头教程的第4章介绍了AEM可编辑模板在AEM内容服务环境中的作用。 可编辑模板用于定义AEM Content Services将最终公开的JSON内容结构。
seo-description: AEM无头教程的第4章介绍了AEM可编辑模板在AEM内容服务环境中的作用。 可编辑模板用于定义AEM Content Services将最终公开的JSON内容结构。
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '835'
ht-degree: 0%

---


# 第4章——定义内容服务模板

AEM无头教程的第4章介绍了AEM可编辑模板在AEM内容服务环境中的作用。 可编辑模板用于定义AEM Content Services通过组合支持Content Services的AEM组件向客户公开的JSON内容结构。

## 了解模板在AEM Content Services中的角色

AEM可编辑模板用于定义将被访问以将事件内容显示为JSON的HTTP端点。

通常，AEM可编辑模板用于定义网页，但这种使用只是惯例。 可编辑的模板可用于构 **建任** 何内容集；如何访问该内容：作为浏览器中的HTML,JavaScript(AEM SPA编辑器)或移动应用程序使用的JSON是请求该页面的方式的函数。

在AEM Content Services中，可编辑的模板用于定义JSON数据的显示方式。

对于应 [!DNL WKND Mobile] 用程序，我们将创建单个可编辑模板，该模板将用于驱动单个API端点。 虽然此示例简单介绍了AEM Headlers的概念，但您可以创建多个页面（或端点），每个页面都展示不同的内容集，以创建更复杂、组织更好的API。

## 了解API端点

要了解如何构建我们的API端点，并了解应向我们的应用程序展示哪些内 [!DNL WKND Mobile] 容，请让我们重新访问设计。

![事件API页面分解](./assets/chapter-4/design-to-component-mapping.png)

正如我们所看到的，我们有三组逻辑内容要提供给移动应用程序。

1. 徽 **标**
2. 标 **签行**
3. 列表 **事件**

为此，我们可以将这些要求映射到AEM组件(在我们的例子中为AEM WCM核心组件)，以便将必需内容显示为JSON。

1. 徽 **标将** 通过图像组 **件呈现**
2. 标记 **行将通** 过文本组件 **进行显示**
3. 列表的 **事件** 将通过内容片段 **列表组件呈现** ，该组件反过来引用一组事件内容片段。

>[!NOTE]
>
>要支持AEM Content Service的页面和组件的JSON导出，页面和组件必 **须从AEM WCM核心组件派生**。
>
>[AEM WCM核心组件](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) 具有内置功能，可支持导出的页面和组件的标准化JSON模式。 本教程中使用的所有WKND Mobile组件(页面、图像、文本和内容片段列表)均源自AEM WCM核心组件。

## 定义事件API模板

1. 导航到 **[!UICONTROL 工具]>[!UICONTROL 常规]>[!UICONTROL 模]板[!DNL WKND Mobile]**>。

1. 创建模 **[!DNL Events API]** 板：

   1. 点 **[!UICONTROL 按顶部]** 操作栏中的创建
   1. 选择模 **[!DNL WKND Mobile - Empty Page]** 板
   1. 点 **[!UICONTROL 按顶部]** 操作栏中的“下一步”
   1. 在模 **[!DNL Events API]** 板标 [!UICONTROL 题字段中输入]
   1. 点 **[!UICONTROL 按顶部]** 操作栏中的创建
   1. 点按 **[!UICONTROL 打开]** ，打开新模板进行编辑

1. 首先，我们允许需要的三个标识的AEM组件通过编辑根布局 [!UICONTROL 容器的] “策略” [!UICONTROL 来模型内容]。 确保“结 **[!UICONTROL 构]** ”模式处于活动状态，选 **[!DNL Layout Container \[Root\]]**&#x200B;择相应模式，然后点 **[!UICONTROL 击“策略]** ”按钮。
1. 在“属 **[!UICONTROL 性]”>“允[!UICONTROL 许的组件]** ”下搜索 **[!DNL WKND Mobile]**。 允许组件组中的 [!DNL WKND Mobile] 以下组件，以便在API页 [!DNL Events] 上使用。

   * **[!DNL WKND Mobile > Image]**

      * 应用程序的徽标
   * **[!DNL WKND Mobile > Text]**

      * 应用程序的介绍文本
   * **[!DNL WKND Mobile > Content Fragment List]**

      * 可在应用程序中显示的事件类别的列表



1. 完成 **[!UICONTROL 后]** ，点按右上角的完成复选标记。
1. **刷新** 浏览器窗口，可在左边 [!UICONTROL 栏中看到新] 的“允许的组件”列表。
1. 从左边栏的组件查找器中，拖入以下AEM组件：
   1. **[!DNL Image]** for Logo
   2. **[!DNL Text]** 标记行
   3. **[!DNL Content Fragment List]** 对于事件
1. **对于以上每个组件**，选择它们并按“解 **锁** ”按钮。
1. 但是，请确保 **布局容器****已锁定，** 以防止添加其他组件，或者删除这三个组件。
1. 点 **[!UICONTROL 按管理员][!UICONTROL 中的页面信息]** >视图 [!DNL WKND Mobile] ，以返回模板列表。 选择新创建的 **[!DNL Events API]** 模板，然 **[!UICONTROL 后点按]** 顶部操作栏中的启用。

>[!VIDEO](https://video.tv.adobe.com/v/28342/?quality=12&learn=on)

>[!NOTE]
>
> 请注意，用于显示内容的组件会添加到“模板”本身并被锁定。 这允许作者编辑预定义的组件，但不能任意添加或删除组件，因为更改API本身可能会破坏JSON结构的假设并破坏使用的应用程序。 所有API都需要稳定。

## 后续步骤

（可选）通 [过AEM包管理器在AEM作者上安装com.adobe.aem.guides.wknd-mobile.content.chapter](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) -4.zip [内容包](http://localhost:4502/crx/packmgr/index.jsp)。 此包包含教程本章和前几章中概述的配置和内容。

* [第5章——创作内容服务页面](./chapter-5.md)
