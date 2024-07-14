---
title: 第4章 — 定义内容服务模板 — Content Services
description: AEM Headless教程的第4章介绍了AEM可编辑模板在AEM Content Services上下文中的作用。 可编辑模板用于定义AEM Content Services最终公开的JSON内容结构。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: ece0bf0d-c4af-4962-9c00-f2849c2d8f6f
duration: 245
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '778'
ht-degree: 0%

---

# 第4章 — 定义Content Service模板

AEM Headless教程的第4章介绍了AEM可编辑模板在AEM Content Services上下文中的作用。 可编辑模板用于定义AEM Content Services通过组合启用Content Services的AEM组件向客户端公开的JSON内容结构。

## 了解“模板”在AEM Content Services中的角色

AEM可编辑模板用于定义访问的HTTP端点，以将事件内容公开为JSON。

传统上，使用AEM的可编辑模板来定义网页，但只是按惯例使用。 可编辑的模板可用于构成&#x200B;**任意**&#x200B;内容集；内容的访问方式：作为浏览器中的HTML，作为JavaScript (AEM SPA Editor)使用的JSON或移动设备应用程序是该页面请求方式的函数。

在AEM Content Services中，可编辑模板用于定义如何公开JSON数据。

对于[!DNL WKND Mobile]应用程序，我们将创建一个用于驱动单个API端点的可编辑模板。 虽然此示例非常简单以说明AEM Headless的概念，但您可以创建多个页面（或端点），每个页面或端点都会公开不同的内容集，以便创建更复杂、更有条理的API。

## 了解API端点

要了解如何构建API端点，并了解应向我们的[!DNL WKND Mobile]应用程序公开的内容，让我们重新访问设计。

![事件API页面分解](./assets/chapter-4/design-to-component-mapping.png)

如我们所见，我们有三个要提供给移动设备应用程序的逻辑内容集。

1. **徽标**
2. **标记行**
3. **事件**&#x200B;的列表

为此，我们可以将这些要求映射到AEM组件(在本例中为AEM WCM核心组件)，以便以JSON形式公开必需的内容。

1. **徽标**&#x200B;通过&#x200B;**图像组件**&#x200B;显示
2. **标记行**&#x200B;通过&#x200B;**文本组件**&#x200B;显示
3. **事件**&#x200B;的列表通过&#x200B;**内容片段列表组件**&#x200B;显示，该组件又引用了一组事件内容片段。

>[!NOTE]
>
>为了支持AEM Content Service的页面和组件的JSON导出，页面和组件必须&#x200B;**派生自AEM WCM核心组件**。
>
>[AEM WCM核心组件](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components)具有内置功能，可支持导出的页面和组件的标准化JSON架构。 本教程中使用的所有WKND Mobile组件（页面、图像、文本和内容片段列表）均派生自AEM WCM核心组件。

## 定义事件API模板

1. 导航到&#x200B;**[!UICONTROL 工具] > [!UICONTROL 常规] > [!UICONTROL 模板] >[!DNL WKND Mobile]**。

1. 创建&#x200B;**[!DNL Events API]**&#x200B;模板：

   1. 点按顶部操作栏中的&#x200B;**[!UICONTROL 创建]**
   1. 选择&#x200B;**[!DNL WKND Mobile - Empty Page]**&#x200B;模板
   1. 点按顶部操作栏中的&#x200B;**[!UICONTROL 下一步]**
   1. 在[!UICONTROL 模板标题]字段中输入&#x200B;**[!DNL Events API]**
   1. 点按顶部操作栏中的&#x200B;**[!UICONTROL 创建]**
   1. 点按&#x200B;**[!UICONTROL 打开]**&#x200B;打开新模板进行编辑

1. 首先，我们允许通过编辑根[!UICONTROL 布局容器]的[!UICONTROL 策略]为内容建模所需的三个已识别AEM组件。 确保&#x200B;**[!UICONTROL 结构]**&#x200B;模式处于活动状态，选择&#x200B;**[!DNL Layout Container \[Root\]]**，然后点按&#x200B;**[!UICONTROL 策略]**&#x200B;按钮。
1. 在&#x200B;**[!UICONTROL 属性] > [!UICONTROL 允许的组件]**&#x200B;下，搜索&#x200B;**[!DNL WKND Mobile]**。 允许[!DNL WKND Mobile]组件组中的以下组件，以便它们可以在[!DNL Events] API页面上使用。

   * **[!DNL WKND Mobile > Image]**

      * 应用程序的徽标

   * **[!DNL WKND Mobile > Text]**

      * 应用程序的介绍性文本

   * **[!DNL WKND Mobile > Content Fragment List]**

      * 应用程序中可显示的事件类别列表

1. 完成后，点按右上角的&#x200B;**[!UICONTROL 完成]**&#x200B;复选标记。
1. **刷新**&#x200B;浏览器窗口以在左边栏中查看新的[!UICONTROL 允许的组件]列表。
1. 从左边栏中的组件查找器中，拖入以下AEM组件：
   1. 徽标的&#x200B;**[!DNL Image]**
   2. 标记行的&#x200B;**[!DNL Text]**
   3. 事件的&#x200B;**[!DNL Content Fragment List]**
1. **对于上述每个组件**，选择它们并按&#x200B;**解锁**&#x200B;按钮。
1. 但是，请确保&#x200B;**布局容器**&#x200B;为&#x200B;**锁定**&#x200B;以防止添加其他组件，或阻止删除这三个组件。
1. 点按&#x200B;**[!UICONTROL 页面信息] > [!UICONTROL 以管理员身份查看]**&#x200B;以返回[!DNL WKND Mobile]模板列表。 选择新创建的&#x200B;**[!DNL Events API]**&#x200B;模板，然后点按顶部操作栏中的&#x200B;**[!UICONTROL 启用]**。

>[!VIDEO](https://video.tv.adobe.com/v/28342?quality=12&learn=on)

>[!NOTE]
>
> 请注意，用于显示内容的元件已添加到模板本身并锁定。 这是为了让作者编辑预定义的组件，但不要随意添加或删除组件，因为更改API本身可能会破坏有关JSON结构的假设并破坏消费应用程序。 所有API都必须稳定。

## 后续步骤

或者，通过[AEM包管理器](http://localhost:4502/crx/packmgr/index.jsp)在AEM Author上安装[com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)内容包。 此资源包包含本教程及前面章节中概述的配置及内容。

* [第5章 — 创作Content Services页面](./chapter-5.md)
