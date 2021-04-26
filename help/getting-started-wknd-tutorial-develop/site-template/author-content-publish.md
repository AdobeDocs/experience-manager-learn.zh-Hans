---
title: 创作内容和发布更改
seo-title: AEM Sites入门 — 创作内容和发布更改
description: 使用AEMAdobe Experience Manager中的页面编辑器更新网站内容。 了解如何使用组件来促进创作。 了解AEM作者环境和发布应用程序之间的差异，并了解如何将更改发布到实时站点。
sub-product: 站点
version: Cloud Service
type: Tutorial
topic: 内容管理
feature: 核心组件，页面编辑器
role: Developer
level: Beginner
kt: 7497
thumbnail: KT-7497.jpg
translation-type: tm+mt
source-git-commit: 67b7f5ee5fc9e42537a9622922327fb7a456d2bd
workflow-type: tm+mt
source-wordcount: '1306'
ht-degree: 2%

---


# 创作内容和发布更改{#author-content-publish}

>[!CAUTION]
>
> 此处展示的快速站点创建功能将在2021年下半年发布。 相关文档可供预览使用。

了解用户如何更新网站内容非常重要。 在本章中，我们将采用&#x200B;**内容作者**&#x200B;的角色，并对上一章中生成的站点进行一些编辑更新。 在本章的结尾，我们将发布更改，以了解实时站点的更新方式。

## 前提条件 {#prerequisites}

这是一个多部分教程，假定[创建站点](./create-site.md)章中概述的步骤已完成。

## 目标 {#objective}

1. 了解AEM Sites中&#x200B;**Pages**&#x200B;和&#x200B;**Components**&#x200B;的概念。
1. 了解如何更新网站内容。
1. 了解如何将更改发布到实时站点。

## 创建新页面{#create-page}

网站通常会分为多个页面，以形成多页体验。 AEM以相同方式构建内容。 然后，为站点创建新页面。

1. 登录到上一章中使用的AEM **作者**&#x200B;服务。
1. 在AEM开始屏幕中，单击&#x200B;**站点** > **WKND站点** > **英语** > **文章**
1. 在右上角，单击&#x200B;**创建** > **页面**。

   ![创建页面](assets/author-content-publish/create-page-button.png)

   这将显示&#x200B;**创建页面**&#x200B;向导。

1. 选择&#x200B;**文章页面**&#x200B;模板，然后单击&#x200B;**下一页**。

   AEM中的页面是基于页面模板创建的。 将在[页面模板](page-templates.md)一章中详细探讨页面模板。

1. 在&#x200B;**属性**&#x200B;下，输入&#x200B;**标题**&#x200B;的“Hello World”。
1. 将&#x200B;**名称**&#x200B;设置为`hello-world`，然后单击&#x200B;**创建**。

   ![初始页面属性](assets/author-content-publish/initial-page-properties.png)

1. 在对话框弹出窗口中，单击&#x200B;**打开**&#x200B;以打开新创建的页面。

## 创作组件{#author-component}

AEM组件可以视为网页的小模块化构件块。 通过将UI拆分为逻辑块或组件，可以更轻松地进行管理。 要重复使用组件，必须对组件进行配置。 可通过作者对话框实现。

AEM提供一组[核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans)，这些组件可供生产使用。 **核心组件**&#x200B;从[Text](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html)和[Image](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html)等基本元素到更复杂的UI元素（如[Carousel](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html)）。

接下来，让我们使用AEM页面编辑器创作一些组件。

1. 导览至在上一个练习中创建的&#x200B;**Hello World**&#x200B;页面。
1. 确保您处于&#x200B;**编辑**&#x200B;模式，并在左侧边栏中单击&#x200B;**组件**&#x200B;图标。

   ![页面编辑器侧栏](assets/author-content-publish/page-editor-siderail.png)

   这将打开组件库，并列表可在页面上使用的可用组件。

1. 向下滚动和&#x200B;**拖放**&#x200B;页面上的&#x200B;**文本(v2)**&#x200B;组件到页面的主可编辑区域。

   ![拖放文本组件](assets/author-content-publish/drag-drop-text-cmp.png)

1. 单击&#x200B;**Text**&#x200B;组件以突出显示，然后单击&#x200B;**扳手**&#x200B;图标![扳手图标](assets/author-content-publish/wrench-icon.png)以打开组件对话框。 输入一些文本并将更改保存到对话框。

   ![富文本组件](assets/author-content-publish/rich-text-populated-component.png)

   **Text**&#x200B;组件现在应在页面上显示富文本。

1. 重复上述步骤，但将&#x200B;**Image(v2)**&#x200B;组件的实例拖动到页面中除外。 打开&#x200B;**Image**&#x200B;组件的对话框。

1. 在左边栏中，单击&#x200B;**资产**&#x200B;图标![资产图标](assets/author-content-publish/asset-icon.png)，切换至&#x200B;**资产查找器**。
1. **将图像拖** 动并拖动到“组件”对话框中，然后单击“ **** 完成”以保存更改。

   ![将资产添加到对话框](assets/author-content-publish/add-asset-dialog.png)

1. 请注意，页面上有固定的组件，如&#x200B;**Title**、**Navigation**、**Search**。 这些区域将配置为页面模板的一部分，无法在单个页面上进行修改。 下一章将进一步探讨此问题。

随时可以尝试其他组件。 有关每个[核心组件的文档，请访问](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)。 有关[页面创作的详细视频系列，可在此处](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html)找到。

## 发布更新{#publish-updates}

AEM环境在&#x200B;**作者服务**&#x200B;和&#x200B;**发布服务**&#x200B;之间拆分。 在本章中，我们对&#x200B;**作者服务**&#x200B;上的站点进行了多处修改。 为了使站点访客视图更改，我们需要将这些更改发布到&#x200B;**发布服务**。

![高级图](assets/author-content-publish/author-publish-high-level-flow.png)

*从“作者”到“发布”的高级内容流*

**1.** 内容作者对网站内容进行更新。可以预览、查看和批准更新以实时推送。

**2.**&#x200B;内容已发布。可以按需执行发布，也可以计划在将来的日期发布。

**3.** 站点访客将看到发布服务上反映的更改。

### 发布更改

接下来，让我们发布更改。

1. 从AEM开始屏幕中，导航到&#x200B;**站点**&#x200B;并选择&#x200B;**WKND站点**。
1. 单击菜单栏中的&#x200B;**管理发布**。

   ![管理发布](assets/author-content-publish/click-manage-publiciation.png)

   由于这是一个全新的站点，因此我们希望发布所有页面，并且可以使用管理发布向导准确定义需要发布的内容。

1. 在&#x200B;**选项**&#x200B;下，将默认设置保留为&#x200B;**发布**，并将其计划为&#x200B;**Now**。 单击&#x200B;**下一步**。
1. 在&#x200B;**范围**&#x200B;下，选择&#x200B;**WKND站点**，然后单击&#x200B;**包含子项**。 在对话框中，取消选中所有框。 我们要发布整个网站。

   ![更新发布范围](assets/author-content-publish/update-scope-publish.png)

1. 单击&#x200B;**已发布引用**&#x200B;按钮。 在对话框中，验证是否已检查所有内容。 这将包括&#x200B;**基本AEM站点模板**&#x200B;以及站点模板生成的多个配置。 单击&#x200B;**完成**&#x200B;进行更新。

   ![发布引用](assets/author-content-publish/publish-references.png)

1. 最后，单击右上角的&#x200B;**发布**&#x200B;以发布内容。

## 视图发布的内容{#publish}

然后，导航到发布服务以视图更改。

1. 获取发布服务URL的简单方法是复制作者url并将`author`单词替换为`publish`。 例如：

   * **作者 URL** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **发布URL** -  `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. 将`/content/wknd.html`添加到发布URL，使最终URL如下所示：`https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`。

   >[!NOTE]
   >
   > 如果您在[站点创建](create-site.md)期间提供了唯一的名称，请更改`wknd.html`以匹配站点名称。

1. 导航到您应当看到的发布URL，而无需任何AEM创作功能。

   ![发布网站](assets/author-content-publish/publish-url-update.png)

1. 使用&#x200B;**导航**&#x200B;菜单，单击&#x200B;**文章** > **Hello World**&#x200B;导航到之前创建的Hello World页面。
1. 返回至&#x200B;**AEM作者服务**&#x200B;并在页面编辑器中进行一些其他内容更改。
1. 单击&#x200B;**页面属性**&#x200B;图标> **发布页面**，直接在页面编辑器中发布这些更改

   ![发布直接](assets/author-content-publish/page-editor-publish.png)

1. 返回至&#x200B;**AEM发布服务**&#x200B;以视图更改。 您很可能会立即&#x200B;**不**&#x200B;看到更新。 这是因为&#x200B;**AEM Publish Service**&#x200B;包含通过Apache Web服务器和CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html)的[缓存。 默认情况下，HTML内容会缓存约5分钟。

1. 为了测试/调试目的而绕过缓存，只需添加一个查询参数，如`?nocache=true`。 URL应类似于`https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`。 有关[可用缓存策略和配置的详细信息，请访问](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html)。

1. 您还可以在云管理器中找到发布服务的URL。 导航到&#x200B;**Cloud Manager项目** > **环境** > **环境**。

   ![视图 Publish Service](assets/author-content-publish/view-environment-segments.png)

   在&#x200B;**环境区段**&#x200B;下，您可以找到指向&#x200B;**作者**&#x200B;和&#x200B;**发布**&#x200B;服务的链接。

## 恭喜！{#congratulations}

祝贺您，您刚刚创作并发布了对AEM站点的更改！

### 后续步骤{#next-steps}

了解如何创建和修改[页面模板](./page-templates.md)。 了解页面模板与页面之间的关系。 了解如何配置页面模板的策略以提供内容的精细管理和品牌一致性。  将基于Adobe XD的模型创建结构良好的杂志文章模板。
