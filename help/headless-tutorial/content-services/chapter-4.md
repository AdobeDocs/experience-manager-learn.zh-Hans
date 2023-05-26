---
title: 第4章 — 定义Content Services模板 — 内容服务
description: AEM Headless教程的第4章介绍了AEM可编辑模板在AEM Content Services上下文中的作用。 可编辑模板用于定义AEM Content Services最终公开的JSON内容结构。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: ece0bf0d-c4af-4962-9c00-f2849c2d8f6f
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 0%

---

# 第4章 — 定义Content Services模板

AEM Headless教程的第4章介绍了AEM可编辑模板在AEM Content Services上下文中的作用。 可编辑模板用于定义AEM Content Services通过组合启用Content Services的AEM组件向客户端公开的JSON内容结构。

## 了解模板在AEM Content Services中的作用

AEM可编辑模板用于定义访问的HTTP端点，以将事件内容公开为JSON。

传统上，使用AEM可编辑模板来定义网页，但这种用法只是惯例。 可编辑的模板可用于撰写 **任意** 内容集；内容的访问方式：作为浏览器中的HTML，作为JavaScript(AEM SPA编辑器)或移动设备应用程序使用的JSON取决于请求该页面的方式。

在AEM Content Services中，可编辑模板用于定义JSON数据的公开方式。

对于 [!DNL WKND Mobile] 应用程序时，我们将创建一个可编辑的模板，用于驱动单个API端点。 虽然此示例可以简单说明AEM Headless的概念，但您可以创建多个页面（或端点），每个页面或端点都会公开不同的内容集，以便创建更复杂、更有条理的API。

## 了解API端点

了解如何构建API端点，以及应向我们的网站展示哪些内容 [!DNL WKND Mobile] 应用程序，让我们重新访问设计。

![事件API页面分解](./assets/chapter-4/design-to-component-mapping.png)

正如我们所看到的，我们有三个逻辑内容集要提供给移动设备应用程序。

1. 此 **徽标**
2. 此 **标记行**
3. 列表 **事件**

为此，我们可以将这些要求映射到AEM组件(在本例中为AEM WCM核心组件)，以便以JSON形式公开必需的内容。

1. 此 **徽标** 是通过 **图像组件**
2. 此 **标记行** 是通过 **文本组件**
3. 列表 **事件** 是通过 **内容片段列表组件** 这进而会引用一组事件内容片段。

>[!NOTE]
>
>要支持AEM Content Service的页面和组件的JSON导出，页面和组件必须 **从AEM WCM核心组件派生**.
>
>[AEM WCM核心组件](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components) 具有内置功能，可支持导出的页面和组件的标准化JSON架构。 本教程中使用的所有WKND Mobile组件（页面、图像、文本和内容片段列表）均派生自AEM WCM核心组件。

## 定义事件API模板

1. 导航到 **[!UICONTROL 工具] > [!UICONTROL 常规] > [!UICONTROL 模板] >[!DNL WKND Mobile]**.

1. 创建 **[!DNL Events API]** 模板：

   1. 点按 **[!UICONTROL 创建]** 在顶部操作栏中
   1. 选择 **[!DNL WKND Mobile - Empty Page]** 模板
   1. 点按 **[!UICONTROL 下一个]** 在顶部操作栏中
   1. 输入 **[!DNL Events API]** 在 [!UICONTROL 模板标题] 字段
   1. 点按 **[!UICONTROL 创建]** 在顶部操作栏中
   1. 点按 **[!UICONTROL 打开]** 打开新模板进行编辑

1. 首先，我们允许我们通过编辑内容来为内容建模所需的三个已识别的AEM组件 [!UICONTROL 策略] 根目录的 [!UICONTROL 布局容器]. 确保 **[!UICONTROL 结构]** 模式处于活动状态，请选择 **[!DNL Layout Container \[Root\]]**，然后点按 **[!UICONTROL 策略]** 按钮。
1. 下 **[!UICONTROL 属性] > [!UICONTROL 允许的组件]** 搜索 **[!DNL WKND Mobile]**. 允许以下组件来自 [!DNL WKND Mobile] 组件组，以便它们可用于 [!DNL Events] API页面。

   * **[!DNL WKND Mobile > Image]**

      * 应用程序的徽标
   * **[!DNL WKND Mobile > Text]**

      * 应用程序的介绍性文本
   * **[!DNL WKND Mobile > Content Fragment List]**

      * 应用程序中可显示的事件类别列表



1. 点按 **[!UICONTROL 完成]** 完成后，右上角的check-mark。
1. **刷新** 要查看的浏览器窗口 [!UICONTROL 允许的组件] 左侧边栏中的列表。
1. 从左边栏中的组件查找器中，拖入以下AEM组件：
   1. **[!DNL Image]** 对于徽标
   2. **[!DNL Text]** （对于标记行）
   3. **[!DNL Content Fragment List]** 事件
1. **对于上述每个组件**，选择它们并按 **解锁** 按钮。
1. 但是，请确保 **布局容器** 是 **已锁定** 以阻止添加其他组件，或阻止删除这三个组件。
1. 点按 **[!UICONTROL 页面信息] > [!UICONTROL 以管理员身份查看]** 以返回 [!DNL WKND Mobile] 模板列表。 选择新创建的 **[!DNL Events API]** 模板并点按 **[!UICONTROL 启用]** 在顶部操作栏中。

>[!VIDEO](https://video.tv.adobe.com/v/28342?quality=12&learn=on)

>[!NOTE]
>
> 请注意，用于显示内容的元件已添加到模板本身中，并被锁定。 这是为了让作者能够编辑预定义的组件，但不能随意添加或删除组件，因为更改API本身可能会破坏有关JSON结构的假设并破坏消费应用程序。 所有API都必须稳定。

## 后续步骤

（可选）安装 [com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) AEM作者上的内容包，通过 [AEM包管理器](http://localhost:4502/crx/packmgr/index.jsp). 此资源包包含本教程及前面章节中概述的配置和内容。

* [第5章 — 创作Content Services页面](./chapter-5.md)
