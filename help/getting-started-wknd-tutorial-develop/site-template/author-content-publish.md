---
title: 创作和发布简介 | AEM快速网站创建
description: 使用AEM的Adobe Experience Manager中的页面编辑器更新网站的内容。 了解如何使用组件来促进创作。 了解AEM创作和发布环境之间的差异，并了解如何将更改发布到实时站点。
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
kt: 7497
thumbnail: KT-7497.jpg
exl-id: 17ca57d1-2b9a-409c-b083-398d38cd6a19
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '1330'
ht-degree: 3%

---

# 创作和发布简介 {#author-content-publish}

了解用户如何更新网站的内容非常重要。 在本章中，我们将采用 **内容作者** 并对上一章中生成的网站进行一些编辑更新。 在章节末尾，我们将发布更改，以了解实时网站的更新方式。

## 前提条件 {#prerequisites}

这是一个多部分教程，我们假定在 [创建网站](./create-site.md) 章节已完成。

## 目标 {#objective}

1. 了解 **页面** 和 **组件** 在AEM Sites。
1. 了解如何更新网站的内容。
1. 了解如何将更改发布到实时站点。

## 创建新页面 {#create-page}

网站通常会划分为多个页面以形成多页面体验。 AEM以相同方式构建内容。 接下来，为网站创建新页面。

1. 登录AEM **作者** 上一章中使用的服务。
1. 在AEM开始屏幕中，单击 **站点** > **WKND站点** > **英语** > **文章**
1. 在右上角，单击 **创建** > **页面**.

   ![创建页面](assets/author-content-publish/create-page-button.png)

   这将显示 **创建页面** 向导。

1. 选择 **文章页面** 模板，单击 **下一个**.

   AEM中的页面是基于页面模板创建的。 页面模板在 [页面模板](page-templates.md) 章节。

1. 在 **属性** 输入 **标题** 《你好世界》中。
1. 设置 **名称** 为 `hello-world` 单击 **创建**.

   ![初始页面属性](assets/author-content-publish/initial-page-properties.png)

1. 在对话框弹出窗口中，单击 **打开** 打开新创建的页面。

## 创作组件 {#author-component}

AEM组件可以视为网页的小模块化构建块。 通过将UI划分为逻辑块或组件，可以更轻松地进行管理。 要重复使用组件，必须对组件进行配置。 此操作可通过创作对话框完成。

AEM提供 [核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans) 生产就绪，可供使用。 的 **核心组件** 范围包括基本元素，如 [文本](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html) 和 [图像](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html) 更复杂的UI元素(如 [轮播](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html).

接下来，使用AEM页面编辑器创作一些组件。

1. 导航到 **《你好世界》** 页面。
1. 确保您位于 **编辑** 模式，并在左侧边栏中单击 **组件** 图标。

   ![页面编辑器侧边栏](assets/author-content-publish/page-editor-siderail.png)

   这将打开组件库，并列出可在页面上使用的可用组件。

1. 向下滚动并 **拖放** a **文本(v2)** 组件添加到页面的主可编辑区域。

   ![拖放文本组件](assets/author-content-publish/drag-drop-text-cmp.png)

1. 单击 **文本** 组件，然后单击 **扳手** 图标 ![扳手图标](assets/author-content-publish/wrench-icon.png) 以打开组件的对话框。 输入一些文本，然后将更改保存到对话框。

   ![富文本组件](assets/author-content-publish/rich-text-populated-component.png)

   的 **文本** 组件现在应在页面上显示富文本。

1. 重复上述步骤，但请拖动 **图像(v2)** 组件。 打开 **图像** 组件对话框。

1. 在左边栏中，切换到 **资产查找器** 单击 **资产** 图标 ![资产图标](assets/author-content-publish/asset-icon.png).
1. **拖放** 将图像放入组件的对话框中，然后单击 **完成** 以保存更改。

   ![将资产添加到对话框](assets/author-content-publish/add-asset-dialog.png)

1. 请注意页面上存在一些组件，如 **标题**, **导航**, **搜索** 已修复。 这些区域将配置为页面模板的一部分，并且无法在单个页面上修改。 下一章将对此进行更多探讨。

您可以随意试用其他一些组件。 有关每个 [核心组件可在此处找到](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html). 有关 [页面创作可在此处找到](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html).

## 发布更新 {#publish-updates}

AEM环境在 **创作服务** 和 **发布服务**. 在本章中，我们对 **创作服务**. 为了让网站访客查看所需的更改，我们需要将这些更改发布到 **发布服务**.

![概要图](assets/author-content-publish/author-publish-high-level-flow.png)

*从创作到发布的内容流程*

**1.** 内容作者对网站内容进行了更新。 更新可以进行预览、审核和批准，以便实时推送。

**2.**&#x200B;内容已发布。可以按需执行发布，也可以计划在将来的日期发布。

**3.** 网站访客将看到发布服务中反映的更改。

### 发布更改

接下来，让我们发布更改。

1. 从AEM开始屏幕中，导航到 **站点** ，然后选择 **WKND站点**.
1. 单击 **管理发布** 中。

   ![管理发布](assets/author-content-publish/click-manage-publiciation.png)

   由于这是一个全新的网站，因此我们希望发布所有页面，并且可以使用“管理发布”向导准确定义需要发布的内容。

1. 在 **选项** 将默认设置保留为 **发布** 并安排 **现在**. 单击&#x200B;**下一步**。
1. 在 **范围**，选择 **WKND站点** 单击 **包含子项设置**. 在对话框中，勾选 **包括子项**. 取消选中其余复选框，以确保整个网站已发布。

   ![更新发布范围](assets/author-content-publish/update-scope-publish.png)

1. 单击 **发布的引用** 按钮。 在对话框中，确认勾选了所有内容。 这将包括 **标准网站模板** 和网站模板生成的多个配置。 单击 **完成** 更新。

   ![发布引用](assets/author-content-publish/publish-references.png)

1. 最后，勾选旁边的框 **WKND站点** 单击 **下一个** 在右上角。
1. 在 **工作流** 步骤，输入 **工作流标题**. 这可以是任何文本，稍后可作为审核跟踪的一部分使用。 输入“初始发布”并单击 **发布**.

![工作流步骤初始发布](assets/author-content-publish/workflow-step-publish.png)

## 查看已发布的内容 {#publish}

接下来，导航到发布服务以查看更改。

1. 获取发布服务URL的简便方法是复制作者URL并替换 `author` word `publish`. 例如：

   * **作者 URL** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **发布URL** - `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. 添加 `/content/wknd.html` 到发布URL，以便最终URL如下所示： `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`.

   >[!NOTE]
   >
   > 更改 `wknd.html` 以匹配您的网站名称(如果在 [网站创建](create-site.md).

1. 导航到发布URL时，您应会看到该站点，但没有任何AEM创作功能。

   ![发布的网站](assets/author-content-publish/publish-url-update.png)

1. 使用 **导航** 菜单单击 **文章** > **《你好世界》** 导航到之前创建的Hello World页面。
1. 返回到 **AEM创作服务** 和在页面编辑器中进行一些其他内容更改。
1. 通过单击 **页面属性** 图标> **发布页面**

   ![发布直接](assets/author-content-publish/page-editor-publish.png)

1. 返回到 **AEM发布服务** 查看更改。 您很可能会 **not** 立即查看更新。 这是因为 **AEM发布服务** 包括 [通过Apache Web服务器和CDN进行缓存](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html). 默认情况下，HTML内容会缓存约5分钟。

1. 要绕过缓存进行测试/调试，只需添加查询参数，如 `?nocache=true`. URL将如下所示 `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`. 有关可用缓存策略和配置的更多详细信息 [可在此处找到](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html).

1. 您还可以在Cloud Manager中找到发布服务的URL。 导航到 **Cloud Manager计划** > **环境** > **环境**.

   ![查看发布服务](assets/author-content-publish/view-environment-segments.png)

   在 **环境区段** 您可以找到指向 **作者** 和 **发布** 服务。

## 恭喜！ {#congratulations}

恭喜，您刚刚创作并发布了对AEM网站的更改！

### 后续步骤 {#next-steps}

在真实实施中，规划网站模型和UI设计通常先于网站创建。 了解如何使用Adobe XD UI工具包在 [使用Adobe XD规划UI](./ui-planning-adobe-xd.md).

是否要继续探索AEM Sites功能？ 你可以直接跳到 [页面模板](./page-templates.md) 以了解页面模板与页面之间的关系。


