---
title: 第2章 — 定义事件内容片段模型 — 内容服务
seo-title: AEM内容服务入门 — 第2章 — 定义事件内容片段模型
description: AEM无头教程的第2章介绍了如何启用和定义内容片段模型，这些模型用于定义用于创建事件的标准化数据结构和创作界面。
seo-description: AEM无头教程的第2章介绍了如何启用和定义内容片段模型，这些模型用于定义用于创建事件的标准化数据结构和创作界面。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1004'
ht-degree: 7%

---


# 第2章 — 使用内容片段模型

AEM内容片段模型定义内容模式，这些内容片段可用于模板AEM作者创建原始内容。 此方法类似于基架或基于表单的创作。 内容片段的关键概念是创作的内容不可知表示形式，这意味着创作内容的用途是多渠道使用，而使用该功能的应用程序(无论是AEM、单页应用程序还是移动应用程序)控制内容向用户显示的方式。

内容片段的主要关注点是确保：

1. 正确的内容是从作者处收集的
2. 内容可以以结构化、易于理解的格式公开给消费应用。

本章介绍启用和定义内容片段模型，用于定义用于建模和创建“事件”的标准化数据结构和创作界面。

## 启用内容片段模型

必须通过&#x200B;**[AEM [!UICONTROL 配置浏览器]](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/configurations.html)**&#x200B;启用内容片段模型&#x200B;****。

如果未为配置启用&#x200B;****&#x200B;内容片段模型，则对于相关AEM配置，将不显示&#x200B;**[!UICONTROL 创建] > [!UICONTROL 内容片段]**&#x200B;按钮。

>[!NOTE]
>
>AEM配置表示存储在`/conf`下的一组[上下文感知租户配置](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html)。 通常，AEM配置与AEM Sites中管理的特定网站或负责子集内容（资产、页面等）的业务单元关联 在AEM中。
>
>为了使配置影响内容层次结构，必须通过该内容层次结构上的`cq:conf`属性引用该配置。 （这是为下面&#x200B;**步骤5**&#x200B;中的[!DNL WKND Mobile]配置实现的）。
>
>使用`global`配置时，该配置将应用于所有内容，而且不需要设置`cq:conf`。
>
>有关详细信息，请参阅[[!UICONTROL 配置浏览器]文档](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/configurations.html)。

1. 以具有相应权限以用户身份登录到AEM作者，以修改相关配置。
   * 在本教程中，可以使用&#x200B;**admin**&#x200B;用户。
1. 导航到&#x200B;**[!UICONTROL 工具] > [!UICONTROL 常规] > [!UICONTROL 配置浏览器]**
1. 点按&#x200B;**[!DNL WKND Mobile]**&#x200B;旁边的&#x200B;**文件夹图标**&#x200B;以进行选择，然后点按左上角的&#x200B;**[!UICONTROL 编辑]按钮**。
1. 选择&#x200B;**[!UICONTROL 内容片段模型]**，然后点按右上角的&#x200B;**[!UICONTROL 保存并关闭]**。

   这允许在应用了[!DNL WKND Mobile]配置的资产文件夹内容树上启用内容片段模型。

   >[!NOTE]
   >
   >此配置更改在[!UICONTROL AEM Configuration] Web UI中不可撤消。 要撤消此配置，请执行以下操作：
   >    
   >    1. 打开[CRXDE Lite](http://localhost:4502/crx/de)
   >    1. 导航至 `/conf/wknd-mobile/settings/dam/cfm`
   >    1. 删除`models`节点

   >    
   >在此配置下创建的任何现有内容片段模型都将被删除，并且其定义存储在`/conf/wknd-mobile/settings/dam/cfm/models`下。

1. 将&#x200B;**[!DNL WKND Mobile]**&#x200B;配置应用到&#x200B;**[!DNL WKND Mobile]资产文件夹**，以允许在该资产文件夹层次结构中创建内容片段模型中的内容片段：

   1. 导航到&#x200B;**[!UICONTROL AEM] > [!UICONTROL 资产] > [!UICONTROL 文件]**
   1. 选择&#x200B;**[!UICONTROL WKND Mobile]文件夹**
   1. 点按顶部操作栏中的&#x200B;**[!UICONTROL 属性]**&#x200B;按钮以打开[!UICONTROL 文件夹属性]
   1. 在[!UICONTROL 文件夹属性]中，点按&#x200B;**[!UICONTROL Cloud Services]**&#x200B;选项卡
   1. 验证“**[!UICONTROL 云配置]**”字段是否设置为&#x200B;**/conf/wknd-mobile**
   1. 点按右上角的&#x200B;**[!UICONTROL 保存并关闭]**&#x200B;以保留更改

>[!VIDEO](https://video.tv.adobe.com/v/28336/?quality=12&learn=on)

## 了解要创建的内容片段模型

在定义内容片段模型之前，让我们来回顾一下我们将带来的体验，以确保我们捕获所有必要的数据点。 为此，我们将审阅移动应用程序设计并将设计元素映射到要收集的内容。

我们可以按如下方式划分定义事件的数据点：

![创建内容片段模型](assets/chapter-2/design-to-model-mapping.png)

借助映射，我们可以定义用于收集和最终公开事件数据的内容片段。

## 创建内容片段模型

1. 导航到&#x200B;**[!UICONTROL 工具] > [!UICONTROL 资产] > [!UICONTROL 内容片段模型]**。
1. 点按要打开的&#x200B;**[!DNL WKND Mobile]**&#x200B;文件夹。
1. 点按&#x200B;**[!UICONTROL 创建]**&#x200B;以打开内容片段模型创建向导。
1. 输入&#x200B;**[!DNL Event]**&#x200B;作为&#x200B;**[!UICONTROL 模型标题]** *(description is optional)*，然后点按&#x200B;**[!UICONTROL 创建]**&#x200B;进行保存。

>[!VIDEO](https://video.tv.adobe.com/v/28337/?quality=12&learn=on)

## 定义内容片段模型的结构

1. 导航到&#x200B;**[!UICONTROL 工具] > [!UICONTROL 资产] > [!UICONTROL 内容片段模型] >[!DNL WKND]**。
1. 选择&#x200B;**[!DNL Event]**&#x200B;内容片段模型，然后点按顶部操作栏中的&#x200B;**[!UICONTROL 编辑]**。
1. 从右侧的&#x200B;**[!UICONTROL 数据类型]选项卡**&#x200B;中，将&#x200B;**[!UICONTROL 单行文本输入]**&#x200B;拖入左下拉区域以定义&#x200B;**[!DNL Question]**&#x200B;字段。
1. 确保在左侧选择新的&#x200B;**[!UICONTROL 单行文本输入]**，并在右侧选择&#x200B;**[!UICONTROL 属性]选项卡**。 按如下方式填充“属性”字段：

   * [!UICONTROL 呈现为] : `textfield`
   * [!UICONTROL 字段标签] : `Event Title`
   * [!UICONTROL 属性名称] : `eventTitle`
   * [!UICONTROL 最大长度] :25
   * [!UICONTROL 必需] :  `Yes`

使用下面定义的输入定义重复这些步骤，以创建事件内容片段模型的其余部分。

>[!NOTE]
>
> **属性名称**&#x200B;字段必须完全匹配，因为Android应用程序已编程为键出这些名称。

### 事件描述

* [!UICONTROL 数据类型] : `Multi-line text`
* [!UICONTROL 字段标签] : `Event Description`
* [!UICONTROL 属性名称] : `eventDescription`
* [!UICONTROL 默认类型] : `Rich text`

### 事件日期和时间

* [!UICONTROL 数据类型] : `Date and time`
* [!UICONTROL 字段标签] : `Event Date and Time`
* [!UICONTROL 属性名称] : `eventDateAndTime`
* [!UICONTROL 必需] :  `Yes`

### 事件类型

* [!UICONTROL 数据类型] : `Enumeration`
* [!UICONTROL 字段标签] : `Event Type`
* [!UICONTROL 属性名称] : `eventType`
* [!UICONTROL 选项] :  `Art,Music,Performance,Photography`

### 票价

* [!UICONTROL 数据类型] : `Number`
* [!UICONTROL 呈现为] : `numberfield`
* [!UICONTROL 字段标签] : `Ticket Price`
* [!UICONTROL 属性名称] : `eventPrice`
* [!UICONTROL 类型] : `Integer`
* [!UICONTROL 必需] :  `Yes`

### 事件图像

* [!UICONTROL 数据类型] : `Content Reference`
* [!UICONTROL 呈现为] : `contentreference`
* [!UICONTROL 字段标签] : `Event Image`
* [!UICONTROL 属性名称] : `eventImage`
* [!UICONTROL 根路径] : `/content/dam/wknd-mobile/images`
* [!UICONTROL 必需] :  `Yes`

### 地点名称

* [!UICONTROL 数据类型] : `Single-line text`
* [!UICONTROL 呈现为] : `textfield`
* [!UICONTROL 字段标签] : `Venue Name`
* [!UICONTROL 属性名称] : `venueName`
* [!UICONTROL 最大长度] :20
* [!UICONTROL 必需] :  `Yes`

### 韦纳城

* [!UICONTROL 数据类型] : `Enumeration`
* [!UICONTROL 字段标签] : `Venue City`
* [!UICONTROL 属性名称] : `venueCity`
* [!UICONTROL 选项] :  `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335/?quality=12&learn=on)

>[!NOTE]
>
>**[!UICONTROL 属性名称]**&#x200B;表示&#x200B;**和**&#x200B;存储此值的JCR属性名称以及JSON文件中的键。 这应该是一个语义名称，在内容片段模型的生命周期中不会更改。

在完成内容片段模型的创建后，您应该最终得到如下定义：


![事件内容片段模型](assets/chapter-2/event-content-fragment-model.png)

## 下一步

或者，通过[AEM包管理器](http://localhost:4502/crx/packmgr/index.jsp)在AEM作者上安装[com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)内容包。 此包包含教程本部分概述的配置和内容。

* [第3章 — 创作事件内容片段](./chapter-3.md)
