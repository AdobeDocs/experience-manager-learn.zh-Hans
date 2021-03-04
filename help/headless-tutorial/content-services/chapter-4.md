---
title: 第4章 — 定义内容服务模板 — 内容服务
seo-title: AEM无外设入门 — 第4章 — 定义内容服务模板
description: AEM无头教程的第4章介绍了AEM可编辑模板在AEM内容服务上下文中的作用。 可编辑模板用于定义AEM Content Services最终将公开的JSON内容结构。
seo-description: AEM无头教程的第4章介绍了AEM可编辑模板在AEM内容服务上下文中的作用。 可编辑模板用于定义AEM Content Services最终将公开的JSON内容结构。
feature: 内容片段、API
topic: 无头、内容管理
role: 开发人员
level: 初学者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '845'
ht-degree: 0%

---


# 第4章 — 定义内容服务模板

AEM无头教程的第4章介绍了AEM可编辑模板在AEM内容服务上下文中的作用。 可编辑模板用于定义AEM Content Services通过启用了Content Services的AEM组件的组合向客户公开的JSON内容结构。

## 了解模板在AEM Content Services中的作用

AEM可编辑模板用于定义将用来将事件内容显示为JSON的HTTP端点。

传统上，AEM可编辑模板用于定义网页，但这种用法只是惯例。 可编辑的模板可用于合成&#x200B;**任何**&#x200B;内容集；如何访问该内容：作为浏览器中的HTML，JavaScript(AEM SPA编辑器)或移动应用程序使用的JSON取决于请求该页面的方式。

在AEM Content Services中，可编辑模板用于定义JSON数据的显示方式。

对于[!DNL WKND Mobile]应用程序，我们将创建一个可编辑模板，用于驱动单个API端点。 虽然此示例简单地说明了AEM无头的概念，但您可以创建多个页面（或端点），每个页面展示不同的内容集以创建更复杂、组织更好的API。

## 了解API端点

要了解如何编写API端点，并了解应向[!DNL WKND Mobile]应用程序公开哪些内容，请允许我们重新访问设计。

![事件API页面分解](./assets/chapter-4/design-to-component-mapping.png)

如我们所见，我们有三组逻辑内容要提供给移动应用程序。

1. **徽标**
2. **标记行**
3. **事件**&#x200B;的列表

为此，我们可以将这些要求映射到AEM组件(在我们的例子中为AEM WCM核心组件)，以便将必需内容显示为JSON。

1. 将通过&#x200B;**图像组件**&#x200B;呈现&#x200B;**Logo**
2. **标记行**&#x200B;将通过&#x200B;**文本组件**&#x200B;显示
3. **事件**&#x200B;的列表将通过&#x200B;**内容片段列表组件**&#x200B;进行呈现，该组件又引用一组事件内容片段。

>[!NOTE]
>
>要支持AEM Content Service的页面和组件的JSON导出，页面和组件必须&#x200B;**从AEM WCM核心组件**&#x200B;派生。
>
>[AEM WCM核心组](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) 件提供内置功能，支持导出的页面和组件的标准化JSON模式。本教程中使用的所有WKND Mobile组件(页面、图像、文本和内容片段列表)均源自AEM WCM核心组件。

## 定义事件API模板

1. 导航到&#x200B;**[!UICONTROL 工具] > [!UICONTROL 常规] > [!UICONTROL 模板] >[!DNL WKND Mobile]**。

1. 创建&#x200B;**[!DNL Events API]**&#x200B;模板：

   1. 点按顶部操作栏中的&#x200B;**[!UICONTROL 创建]**
   1. 选择&#x200B;**[!DNL WKND Mobile - Empty Page]**&#x200B;模板
   1. 点按顶部操作栏中的&#x200B;**[!UICONTROL 下一个]**
   1. 在[!UICONTROL 模板标题]字段中输入&#x200B;**[!DNL Events API]**
   1. 点按顶部操作栏中的&#x200B;**[!UICONTROL 创建]**
   1. 点按&#x200B;**[!UICONTROL 打开]**&#x200B;打开要编辑的新模板

1. 首先，我们允许需要的三个标识的AEM组件通过编辑根[!UICONTROL 布局容器]的[!UICONTROL Policy]来对内容进行建模。 确保&#x200B;**[!UICONTROL Structure]**&#x200B;模式处于活动状态，选择&#x200B;**[!DNL Layout Container \[Root\]]**，然后点按&#x200B;**[!UICONTROL Policy]**&#x200B;按钮。
1. 在&#x200B;**[!UICONTROL 属性] > [!UICONTROL 允许的组件]**&#x200B;下搜索&#x200B;**[!DNL WKND Mobile]**。 允许[!DNL WKND Mobile]组件组中的以下组件，以便在[!DNL Events] API页上使用它们。

   * **[!DNL WKND Mobile > Image]**

      * 应用程序的徽标
   * **[!DNL WKND Mobile > Text]**

      * 应用程序的介绍文本
   * **[!DNL WKND Mobile > Content Fragment List]**

      * 可在应用程序中显示的事件类别列表



1. 完成后，点按右上角的&#x200B;**[!UICONTROL 完成]**&#x200B;复选标记。
1. **请** 刷新浏览器窗口，在左边 [!UICONTROL 栏] 中查看新允许的组件列表。
1. 从左边栏的组件查找器中，拖入以下AEM组件：
   1. **[!DNL Image]** 的
   2. **[!DNL Text]** 标签行
   3. **[!DNL Content Fragment List]** 对于事件
1. **对于上述每个组件**，选择它们并按“解锁 **** ”按钮。
1. 但是，请确保&#x200B;**布局容器**&#x200B;为&#x200B;**锁定**&#x200B;以阻止添加其他组件，或者删除这三个组件。
1. 点按Admin ]**中的**[!UICONTROL &#x200B;页面信息] > [!UICONTROL 视图，返回至[!DNL WKND Mobile]模板列表。 选择新创建的&#x200B;**[!DNL Events API]**&#x200B;模板，然后点按顶部操作栏中的&#x200B;**[!UICONTROL 启用]**。

>[!VIDEO](https://video.tv.adobe.com/v/28342/?quality=12&learn=on)

>[!NOTE]
>
> 请注意，用于显示内容的组件会添加到“模板”本身并被锁定。 这是为了允许作者编辑预定义的组件，但不能任意添加或删除组件，因为更改API本身可能会破坏JSON结构的假设并破坏使用的应用程序。 所有API都需要稳定。

## 后续步骤

或者，也可以通过[AEM包管理器](http://localhost:4502/crx/packmgr/index.jsp)在AEM作者上安装[com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)内容包。 此包包含教程本章和前几章中概述的配置和内容。

* [第5章 — 创作内容服务页面](./chapter-5.md)
