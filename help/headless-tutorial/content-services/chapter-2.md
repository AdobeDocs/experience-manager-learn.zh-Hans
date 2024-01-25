---
title: 第2章 — 定义事件内容片段模型 — 内容服务
description: AEM Headless教程的第2章涵盖了启用和定义内容片段模型，这些模型用于定义规范化的数据结构和用于创建事件的创作界面。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 8b05fc02-c0c5-48ad-a53e-d73b805ee91f
duration: 421
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '940'
ht-degree: 1%

---

# 第2章 — 使用内容片段模型

AEM内容片段模型定义了内容架构，可用于对AEM作者创建的原始内容进行模板化。 这种方法类似于基架或基于表单的创作。 内容片段的关键概念是创作的内容与呈现无关，这意味着它可用于多渠道使用，即AEM、单页应用程序或移动设备应用程序等消费应用程序控制向用户显示内容的方式。

内容片段的主要问题是确保：

1. 从作者处收集正确的内容
2. 内容可以以结构化、易于理解的格式向消费应用程序公开。

本章介绍如何启用和定义内容片段模型，这些模型用于定义规范化的数据结构和创作界面，以便建模和创建“事件”。

## 启用内容片段模型

内容片段模型 **必须** 通过以下方式启用 **[AEM [!UICONTROL 配置浏览器]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html)**.

如果内容片段模型为 **非** 已为配置启用， **[!UICONTROL 创建] > [!UICONTROL 内容片段]** 按钮将不会针对相关的AEM配置显示。

>[!NOTE]
>
>AEM配置表示一组 [上下文感知租户配置](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) 存储于 `/conf`. 通常AEM配置与AEM Sites中管理的特定Web站点或负责内容子集（资产、页面等）的业务部门相关联 在AEM中。
>
>为了使配置影响内容层次结构，必须通过引用配置 `cq:conf` 属性。 (这是为实现 [!DNL WKND Mobile] 中的配置 **步骤5** 下)。
>
>当 `global` 使用配置，该配置适用于所有内容，并且 `cq:conf` 无需设置。
>
>请参阅 [[!UICONTROL 配置浏览器] 文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html) 以了解更多信息。

1. 以具有适当权限的用户身份登录AEM Author以修改相关配置。
   * 在本教程中， **管理员** 可以使用用户。
1. 导航到 **[!UICONTROL 工具] > [!UICONTROL 常规] > [!UICONTROL 配置浏览器]**
1. 点按 **文件夹图标** 旁边 **[!DNL WKND Mobile]** 以选择，然后点按 **[!UICONTROL 编辑] 按钮** 左上角。
1. 选择 **[!UICONTROL 内容片段模型]**，然后点击 **[!UICONTROL 保存并关闭]** 在右上角。

   这将启用具有 [!DNL WKND Mobile] 已应用配置。

   >[!NOTE]
   >
   >此配置更改不可从 [!UICONTROL AEM配置] Web UI。 要撤消此配置，请执行以下操作：
   >    
   >    1. 打开 [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. 导航到 `/conf/wknd-mobile/settings/dam/cfm`
   >    1. 删除 `models` 节点
   >    
   >在此配置下创建的任何现有内容片段模型都将被删除，并且其定义将存储在中 `/conf/wknd-mobile/settings/dam/cfm/models`.

1. 应用 **[!DNL WKND Mobile]** 配置到 **[!DNL WKND Mobile]资产文件夹** 要允许在该资产文件夹层次结构中创建内容片段模型中的内容片段，请执行以下操作：

   1. 导航到 **[!UICONTROL AEM] > [!UICONTROL 资产] > [!UICONTROL 文件]**
   1. 选择 **[!UICONTROL WKND Mobile] 文件夹**
   1. 点按 **[!UICONTROL 属性]** 顶部操作栏中用于打开的按钮 [!UICONTROL 文件夹属性]
   1. 在 [!UICONTROL 文件夹属性]，点按 **[!UICONTROL Cloud Service]** 选项卡
   1. 验证 **[!UICONTROL 云配置]** 字段设置为 **/conf/wknd-mobile**
   1. 点按 **[!UICONTROL 保存并关闭]** 以保留更改

>[!VIDEO](https://video.tv.adobe.com/v/28336?quality=12&learn=on)

>[!WARNING]
>
> __内容片段模型__ 已移动自 __“工具”>“资产”__ 到 __“工具”>“常规”__.

## 了解要创建的内容片段模型

在定义内容片段模型之前，我们先查看一下将推动的体验，以确保捕获所有必要的数据点。 为此，我们将审查移动设备应用程序设计，并将设计元素映射到要收集的内容。

我们可以按如下方式划分定义事件的数据点：

![创建内容片段模型](assets/chapter-2/design-to-model-mapping.png)

借助映射，我们可以定义用于收集并最终公开事件数据的内容片段。

## 创建内容片段模型

1. 导航到&#x200B;**[!UICONTROL 工具] > [!UICONTROL 常规] > [!UICONTROL 内容片段模型]**。
1. 点按 **[!DNL WKND Mobile]** 要打开的文件夹。
1. 点按 **[!UICONTROL 创建]** 以打开内容片段模型创建向导。
1. 输入 **[!DNL Event]** 作为 **[!UICONTROL 模型标题]** *（说明是可选的）* 并点击 **[!UICONTROL 创建]** 以保存。

>[!VIDEO](https://video.tv.adobe.com/v/28337?quality=12&learn=on)

## 定义内容片段模型的结构

1. 导航到 **[!UICONTROL 工具] > [!UICONTROL 常规] > [!UICONTROL 内容片段模型] >[!DNL WKND]**.
1. 选择 **[!DNL Event]** 内容片段模型和点按 **[!UICONTROL 编辑]** 在顶部操作栏中。
1. 从 **[!UICONTROL 数据类型] 选项卡** 在右侧，将 **[!UICONTROL 单行文本输入]** 放入左侧放置区域以定义 **[!DNL Question]** 字段。
1. 确保新的 **[!UICONTROL 单行文本输入]** 左侧选定的，并且 **[!UICONTROL 属性] 选项卡** ，则会在右侧选中。 按如下方式填充属性字段：

   * [!UICONTROL 呈现为] ： `textfield`
   * [!UICONTROL 字段标签] ： `Event Title`
   * [!UICONTROL 属性名称] ： `eventTitle`
   * [!UICONTROL 最大长度] ：25
   * [!UICONTROL 必填] ： `Yes`

使用下面定义的输入定义重复这些步骤，以创建事件内容片段模型的其余部分。

>[!NOTE]
>
> 此 **属性名称** 字段必须完全匹配，因为Android应用程序经过编程可关闭这些名称。

### 事件描述

* [!UICONTROL 数据类型] ： `Multi-line text`
* [!UICONTROL 字段标签] ： `Event Description`
* [!UICONTROL 属性名称] ： `eventDescription`
* [!UICONTROL 默认类型] ： `Rich text`

### 活动日期和时间

* [!UICONTROL 数据类型] ： `Date and time`
* [!UICONTROL 字段标签] ： `Event Date and Time`
* [!UICONTROL 属性名称] ： `eventDateAndTime`
* [!UICONTROL 必填] ： `Yes`

### 事件类型

* [!UICONTROL 数据类型] ： `Enumeration`
* [!UICONTROL 字段标签] ： `Event Type`
* [!UICONTROL 属性名称] ： `eventType`
* [!UICONTROL 选项] ： `Art,Music,Performance,Photography`

### 票价

* [!UICONTROL 数据类型] ： `Number`
* [!UICONTROL 呈现为] ： `numberfield`
* [!UICONTROL 字段标签] ： `Ticket Price`
* [!UICONTROL 属性名称] ： `eventPrice`
* [!UICONTROL 类型] ： `Integer`
* [!UICONTROL 必填] ： `Yes`

### 事件图像

* [!UICONTROL 数据类型] ： `Content Reference`
* [!UICONTROL 呈现为] ： `contentreference`
* [!UICONTROL 字段标签] ： `Event Image`
* [!UICONTROL 属性名称] ： `eventImage`
* [!UICONTROL 根路径] ： `/content/dam/wknd-mobile/images`
* [!UICONTROL 必填] ： `Yes`

### 地点名称

* [!UICONTROL 数据类型] ： `Single-line text`
* [!UICONTROL 呈现为] ： `textfield`
* [!UICONTROL 字段标签] ： `Venue Name`
* [!UICONTROL 属性名称] ： `venueName`
* [!UICONTROL 最大长度] ：20
* [!UICONTROL 必填] ： `Yes`

### 地点城市

* [!UICONTROL 数据类型] ： `Enumeration`
* [!UICONTROL 字段标签] ： `Venue City`
* [!UICONTROL 属性名称] ： `venueCity`
* [!UICONTROL 选项] ： `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335?quality=12&learn=on)

>[!NOTE]
>
>此 **[!UICONTROL 属性名称]** 表示 **两者** JCR属性名称（此值会存储在该名称中）以及JSON文件中的键。 这应当是一个语义名称，在内容片段模型的生命周期内不会发生更改。

完成内容片段模型的创建后，您应该得到如下定义：


![事件内容片段模型](assets/chapter-2/event-content-fragment-model.png)

## 下一步

（可选）安装 [com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) AEM创作中的内容包，通过 [AEM包管理器](http://localhost:4502/crx/packmgr/index.jsp). 此资源包包含本教程此部分中概述的配置及内容。

* [第3章 — 创作事件内容片段](./chapter-3.md)
