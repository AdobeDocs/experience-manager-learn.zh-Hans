---
title: 创作和发布简介 | AEM快速站点创建
description: 使用Adobe Experience Manager AEM中的页面编辑器来更新网站内容。 了解如何使用组件促进创作。 了解AEM创作环境和发布环境之间的区别，并了解如何将更改发布到实时站点。
version: Cloud Service
type: Tutorial
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
kt: 7497
thumbnail: KT-7497.jpg
exl-id: 17ca57d1-2b9a-409c-b083-398d38cd6a19
recommendations: noDisplay, noCatalog
source-git-commit: de2fa2e4c29ce6db31233ddb1abc66a48d2397a6
workflow-type: tm+mt
source-wordcount: '1330'
ht-degree: 3%

---

# 创作和发布简介 {#author-content-publish}

了解用户将如何更新网站内容非常重要。 在本章中，我们将采用 **内容作者** 并对上一章生成的站点进行编辑更新。 在本章末尾，我们将发布更改以了解如何更新活动站点。

## 前提条件 {#prerequisites}

这是一个多部分教程，并假定中概述的步骤 [创建站点](./create-site.md) 章节已完成。

## 目标 {#objective}

1. 了解 **页面** 和 **组件** 在AEM Sites中。
1. 了解如何更新网站内容。
1. 了解如何将更改发布到实时网站。

## 创建新页面 {#create-page}

网站通常会划分为多个页面，以形成多页面体验。 AEM以相同的方式构建内容。 接下来，为站点创建新页面。

1. 登录到AEM **作者** 上一章中使用的服务。
1. 从AEM开始屏幕，单击 **站点** > **WKND站点** > **英语** > **文章**
1. 在右上角，单击 **创建** > **页面**.

   ![创建页面](assets/author-content-publish/create-page-button.png)

   这将显示 **创建页面** 向导。

1. 选择 **文章页面** 模板并单击 **下一个**.

   AEM中的页面是根据页面模板创建的。 有关页面模板的详细信息，请参阅 [页面模板](page-templates.md) 章节。

1. 下 **属性** 输入 **标题** 《你好，世界》。
1. 设置 **名称** 成为 `hello-world` 并单击 **创建**.

   ![初始页面属性](assets/author-content-publish/initial-page-properties.png)

1. 在对话框弹出窗口中，单击 **打开** 以打开新创建的页面。

## 创作组件 {#author-component}

可以将AEM组件视为网页的小型模块化构建基块。 通过将UI分成逻辑块或组件，可以使其更易于管理。 要重用组件，组件必须可配置。 这是通过“创作”对话框实现的。

AEM提供了一组 [核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans) 即生产就绪，可供使用。 此 **核心组件** 从基本元素开始，例如 [文本](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html) 和 [图像](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html) 到更复杂的UI元素，如 [轮播](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html).

接下来，使用AEM页面编辑器创作一些组件。

1. 导航到 **《你好世界》** 在上一个练习中创建的页面。
1. 确保您位于 **编辑** 模式，然后在左侧边栏中，单击 **组件** 图标。

   ![页面编辑器侧边栏](assets/author-content-publish/page-editor-siderail.png)

   这将打开组件库，并列出可在页面上使用的可用组件。

1. 向下滚动和 **拖放** a **文本(v2)** 组件置于页面的主要可编辑区域。

   ![拖放文本组件](assets/author-content-publish/drag-drop-text-cmp.png)

1. 单击 **文本** 组件高亮显示，然后单击 **扳手** 图标 ![“扳手”图标](assets/author-content-publish/wrench-icon.png) 以打开组件的对话框。 输入一些文本并将更改保存到对话框。

   ![富文本组件](assets/author-content-publish/rich-text-populated-component.png)

   此 **文本** 组件现在应在页面上显示富文本。

1. 重复上述步骤，但拖动 **图像(v2)** 组件放在页面上。 打开 **图像** 组件的对话框。

1. 在左边栏中，切换到 **资产查找器** 单击 **资产** 图标 ![资产图标](assets/author-content-publish/asset-icon.png).
1. **拖放** 将图像放入组件对话框中，然后单击 **完成** 以保存更改。

   ![将资源添加到对话框](assets/author-content-publish/add-asset-dialog.png)

1. 请注意，页面上有一些组件，例如 **标题**， **导航**， **搜索** 已修复。 这些区域配置为页面模板的一部分，无法在单个页面上修改。 下一章将对此进行更多探讨。

您可以随意尝试一些其他组件。 关于每个项目的文档 [可以在此处找到核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans). 有关以下内容的详细视频系列 [可以在此处找到页面创作](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html).

## 发布更新 {#publish-updates}

AEM环境在 **作者服务** 和 **发布服务**. 在本章中，我们对以下网站进行了一些修改： **作者服务**. 为了让网站访客查看更改，我们需要将它们发布到 **发布服务**.

![高级图表](assets/author-content-publish/author-publish-high-level-flow.png)

*从创作到发布的高级别内容流*

**1.** 内容作者对网站内容进行更新。 可以预览、审核和批准更新以将其推送到实时状态。

**2.** 内容已发布。 可按需执行发布，也可计划在未来某个日期发布。

**3.** 网站访客将看到更改反映在Publish服务中。

### 发布更改

接下来，让我们发布更改。

1. 从AEM开始屏幕导航到 **站点** 并选择 **WKND站点**.
1. 单击 **管理发布** 在菜单栏中。

   ![管理发布](assets/author-content-publish/click-manage-publiciation.png)

   由于这是一个全新的站点，因此我们希望发布所有页面，并且可以使用管理发布向导来确切定义需要发布的内容。

1. 下 **选项** 将默认设置保留为 **Publish** 并将其安排在 **现在**. 单击&#x200B;**下一步**。
1. 下 **范围**，选择 **WKND站点** 并单击 **包括子设置**. 在对话框中，选中 **包括子项**. 取消选中其余框以确保发布整个站点。

   ![更新发布范围](assets/author-content-publish/update-scope-publish.png)

1. 单击 **已发布引用** 按钮。 在对话框中，验证是否已选中所有内容。 这将包括 **标准站点模板** 以及站点模板生成的多个配置。 单击 **完成** 以进行更新。

   ![发布引用](assets/author-content-publish/publish-references.png)

1. 最后，选中旁边的框 **WKND站点** 并单击 **下一个** 在右上角。
1. 在 **工作流** 步骤，输入 **工作流标题**. 这可以是任何文本，并且以后可用作审核跟踪的一部分。 输入“Initial publish”并单击 **Publish**.

![工作流步骤初始发布](assets/author-content-publish/workflow-step-publish.png)

## 查看已发布的内容 {#publish}

接下来，导航到Publish服务以查看更改。

1. 获取Publish服务URL的一种简单方法是复制作者URL并替换 `author` 单词替换为 `publish`. 例如：

   * **作者 URL** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **发布URL** - `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. 添加 `/content/wknd.html` 到发布URL，以便最终URL如下所示： `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`.

   >[!NOTE]
   >
   > 更改 `wknd.html` 以匹配网站名称，如果您在 [站点创建](create-site.md).

1. 导航到发布URL，您应该会看到站点，但不具有任何AEM创作功能。

   ![已发布站点](assets/author-content-publish/publish-url-update.png)

1. 使用 **导航** 菜单点击 **文章** > **《你好世界》** 以导航到之前创建的Hello World页面。
1. 返回到 **AEM创作服务** 并在页面编辑器中进行了一些其他内容更改。
1. 通过单击 **页面属性** 图标> **发布页面**

   ![直接发布](assets/author-content-publish/page-editor-publish.png)

1. 返回到 **AEM发布服务** 以查看更改。 最有可能的是 **非** 立即查看更新。 这是因为 **AEM发布服务** 包含 [通过Apache Web Server和CDN进行缓存](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html). 默认情况下，HTML内容的缓存时间约为5分钟。

1. 要绕过缓存以进行测试/调试，只需添加一个查询参数，例如 `?nocache=true`. URL如下所示 `https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`. 有关缓存策略和可用配置的更多详细信息 [可在此处找到](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html).

1. 您还可以在Cloud Manager中找到发布服务的URL。 导航到 **Cloud Manager项目** > **环境** > **环境**.

   ![查看发布服务](assets/author-content-publish/view-environment-segments.png)

   下 **环境区段** 您可以找到指向以下内容的链接： **作者** 和 **Publish** 服务。

## 恭喜！ {#congratulations}

恭喜，您刚才已创作更改并将更改发布到AEM站点！

### 后续步骤 {#next-steps}

在现实实施中，通常在创建站点之前先规划具有模型和UI设计的站点。 了解如何使用Adobe XD UI工具包来设计和加速您在中的Adobe Experience Manager Sites实施 [使用Adobe XD进行UI规划](./ui-planning-adobe-xd.md).

想要继续探索AEM Sites功能？ 请随时跳到上一章 [页面模板](./page-templates.md) 以了解页面模板与页面之间的关系。


