---
title: 第4章 — 定义内容服务模板 — 内容服务
description: AEM Headless教程的第4章介绍了AEM可编辑模板在AEM内容服务上下文中的角色。 可编辑的模板用于定义AEM Content Services最终公开的JSON内容结构。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: ece0bf0d-c4af-4962-9c00-f2849c2d8f6f
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 0%

---

# 第4章 — 定义内容服务模板

AEM Headless教程的第4章介绍了AEM可编辑模板在AEM内容服务上下文中的角色。 可编辑的模板用于定义AEM Content Services通过组合启用了Content Services的AEM组件而向客户公开的JSON内容结构。

## 了解模板在AEM Content Services中的角色

AEM可编辑的模板用于定义用于将事件内容显示为JSON的HTTP端点。

传统上，AEM可编辑的模板用于定义网页，但这种用法只是惯例。 可编辑的模板可用于撰写 **any** 内容集；如何访问该内容：作为浏览器中的HTML,JavaScript(AEM SPA编辑器)或移动设备应用程序使用的JSON取决于该页面的请求方式。

在AEM Content Services中，可编辑的模板用于定义JSON数据的显示方式。

对于 [!DNL WKND Mobile] 应用程序中，我们将创建一个可编辑的模板，用于驱动单个API端点。 尽管此示例简单地说明了AEM Headless的概念，但您可以创建多个页面（或端点），每个页面都公开不同的内容集，以创建更复杂、组织更好的API。

## 了解API端点

了解如何编写API端点，并了解应向我们的 [!DNL WKND Mobile] 应用程序，让我们重新看看设计。

![事件API页面分解](./assets/chapter-4/design-to-component-mapping.png)

如我们所见，我们有三组逻辑内容要提供给移动设备应用程序。

1. 的 **徽标**
2. 的 **标记行**
3. 列表 **事件**

为此，我们可以将这些要求映射到AEM组件(在本例中为AEM WCM核心组件)，以便将必需的内容显示为JSON。

1. 的 **徽标** 通过 **图像组件**
2. 的 **标记行** 通过 **文本组件**
3. 列表 **事件** 通过 **内容片段列表组件** 这反过来又引用了一组事件内容片段。

>[!NOTE]
>
>要支持AEM内容服务将页面和组件的JSON导出，页面和组件必须 **派生自AEM WCM核心组件**.
>
>[AEM WCM核心组件](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) 具有内置功能，可支持导出的页面和组件的标准化JSON模式。 本教程中使用的所有WKND移动设备组件（页面、图像、文本和内容片段列表）均源自AEM WCM核心组件。

## 定义事件API模板

1. 导航到 **[!UICONTROL 工具] > [!UICONTROL 常规] > [!UICONTROL 模板] >[!DNL WKND Mobile]**.

1. 创建 **[!DNL Events API]** 模板：

   1. 点按 **[!UICONTROL 创建]** 在顶部操作栏中
   1. 选择 **[!DNL WKND Mobile - Empty Page]** 模板
   1. 点按 **[!UICONTROL 下一个]** 在顶部操作栏中
   1. 输入 **[!DNL Events API]** 在 [!UICONTROL 模板标题] 字段
   1. 点按 **[!UICONTROL 创建]** 在顶部操作栏中
   1. 点按 **[!UICONTROL 打开]** 打开新模板进行编辑

1. 首先，我们允许我们需要的三个已识别的AEM组件通过编辑 [!UICONTROL 策略] 根 [!UICONTROL 布局容器]. 确保 **[!UICONTROL 结构]** 模式激活时，选择 **[!DNL Layout Container \[Root\]]**，然后点按 **[!UICONTROL 策略]** 按钮。
1. 在 **[!UICONTROL 属性] > [!UICONTROL 允许的组件]** 搜索 **[!DNL WKND Mobile]**. 允许从 [!DNL WKND Mobile] 组件组，以便在 [!DNL Events] API页面。

   * **[!DNL WKND Mobile > Image]**

      * 应用程序的徽标
   * **[!DNL WKND Mobile > Text]**

      * 应用程序的介绍性文本
   * **[!DNL WKND Mobile > Content Fragment List]**

      * 可在应用程序中显示的事件类别列表



1. 点按 **[!UICONTROL 完成]** 完成后，在右上角勾选标记。
1. **刷新** 用于查看新 [!UICONTROL 允许的组件] 列表。
1. 从左边栏的组件查找器中，拖入以下AEM组件：
   1. **[!DNL Image]** 标识
   2. **[!DNL Text]** 标记行
   3. **[!DNL Content Fragment List]** 对于事件
1. **对于上述每个组件**，选择它们并按 **解锁** 按钮。
1. 但是，请确保 **布局容器** is **锁定** 以阻止添加其他组件，或者删除这三个组件。
1. 点按 **[!UICONTROL 页面信息] > [!UICONTROL 在管理员中查看]** 返回 [!DNL WKND Mobile] 模板列表。 选择新创建的 **[!DNL Events API]** 模板和点按 **[!UICONTROL 启用]** 中。

>[!VIDEO](https://video.tv.adobe.com/v/28342/?quality=12&learn=on)

>[!NOTE]
>
> 请注意，用于显示内容的组件会添加到模板本身并被锁定。 这是为了允许作者编辑预定义的组件，但不能任意添加或删除组件，因为更改API本身可能会破坏JSON结构的假设并破坏耗费的应用程序。 所有API都需要稳定。

## 下面的步骤

（可选）安装 [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) AEM作者上的内容包(通过 [AEM包管理器](http://localhost:4502/crx/packmgr/index.jsp). 此包包含教程本章及前几章中概述的配置和内容。

* [第5章 — 创作内容服务页面](./chapter-5.md)
