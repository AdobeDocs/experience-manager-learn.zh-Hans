---
title: 创作和发布简介 | AEM 快速网站创建
description: 使用 Adobe Experience Manager (AEM) 中的页面编辑器更新网站内容。了解如何使用组件来促进创作。了解 AEM 作者与发布两个环境之间的区别，学习如何将更改发布到上线网站。
version: Experience Manager as a Cloud Service
topic: Content Management
feature: Core Components, Page Editor
role: Developer
level: Beginner
jira: KT-7497
thumbnail: KT-7497.jpg
doc-type: Tutorial
exl-id: 17ca57d1-2b9a-409c-b083-398d38cd6a19
recommendations: noDisplay, noCatalog
duration: 263
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1285'
ht-degree: 100%

---

# 创作和发布简介 {#author-content-publish}

了解用户如何更新网站内容非常重要。在本章中，我们将采用&#x200B;**内容作者**&#x200B;的角色，对上一章生成的网站做一些编辑更新。在本章的最后，我们将发布这些更改，以了解如何更新上线网站。

## 先决条件 {#prerequisites}

这是一个多段式教程，并且假设您已完成[创建网站](./create-site.md)一章中概述的步骤。

## 目标 {#objective}

1. 了解 AEM Sites 中&#x200B;**页面**&#x200B;和&#x200B;**组件**&#x200B;的概念。
1. 了解如何更新网站内容。
1. 了解如何将更改发布到上线网站。

## 创建一个新页面 {#create-page}

网站通常分为多个页面，以形成一种多页面体验。AEM 以相同的方式组织内容的结构。接下来，为网站创建一个新页面。

1. 登录上一章中使用的 AEM **作者**&#x200B;服务。
1. 在 AEM 开始屏幕中，点击&#x200B;**网站** > **WKND 网站** > **英语** > **文章**
1. 在右上角点击&#x200B;**创建** > **页面**。

   ![创建页面](assets/author-content-publish/create-page-button.png)

   现在会打开&#x200B;**创建页面**&#x200B;向导。

1. 选择&#x200B;**文章页面**&#x200B;模板，然后点击&#x200B;**下一步**。

   AEM 中的页面是基于页面模板创建的。[页面模板](page-templates.md)一章详细介绍了页面模板。

1. 在&#x200B;**属性**&#x200B;中输入&#x200B;**标题**“Hello World”。
1. 将&#x200B;**名称**&#x200B;设置为 `hello-world`，然后点击&#x200B;**创建**。

   ![初始页面属性](assets/author-content-publish/initial-page-properties.png)

1. 在弹出的对话框中点击&#x200B;**打开**，打开新创建的页面。

## 创作组件 {#author-component}

AEM 组件可以被看作是网页的小型模块化构建基块。将 UI 分解为逻辑分块或组件，可以更轻松地进行管理。为了重复使用组件，组件必须是可配置的。这个操作通过作者对话框进行。

AEM 提供一组可立即用于生产的[核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)。**核心组件**&#x200B;的范围从[文本](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/text.html)和[图像](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/image.html)等基本元素到[轮播](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/carousel.html)等更复杂的 UI 元素。

接下来，使用 AEM 页面编辑器创作几个组件。

1. 导航到上一个练习中创建的 **Hello World** 页面。
1. 确保您处于&#x200B;**编辑**&#x200B;模式，在左侧边栏中点击&#x200B;**组件**&#x200B;图标。

   ![页面编辑器侧边栏](assets/author-content-publish/page-editor-siderail.png)

   现在会打开组件库，其中列出了可用在页面上的可用组件。

1. 向下滚动，将一个&#x200B;**文本 (v2)** 组件&#x200B;**拖放**&#x200B;到页面的主要可编辑区域中。

   ![拖放文本组件](assets/author-content-publish/drag-drop-text-cmp.png)

1. 点击&#x200B;**文本**&#x200B;组件使其突出显示，然后点击&#x200B;**扳手**&#x200B;图标![扳手图标](assets/author-content-publish/wrench-icon.png)，打开组件的对话框。输入一些文本，然后保存对话框的更改。

   ![富文本组件](assets/author-content-publish/rich-text-populated-component.png)

   **文本**&#x200B;组件现在应该在页面上显示富文本。

1. 重复上述步骤，但现在将&#x200B;**图像 (v2)** 组件的一个实例拖到页面上。打开&#x200B;**图像**&#x200B;组件的对话框。

1. 在左边栏中点击&#x200B;**资产**&#x200B;图标![资产图标](assets/author-content-publish/asset-icon.png)，切换到&#x200B;**资产查找器**。
1. 将图像&#x200B;**拖放**&#x200B;到组件的对话框中，然后点击&#x200B;**完成**，保存更改。

   ![在对话框中添加资产](assets/author-content-publish/add-asset-dialog.png)

1. 观察页面上的一些组件，例如&#x200B;**标题**、**导航**、**搜索**，都是固定的。这些区域被配置为页面模板的一部分，不能在单个页面上更改。下一章将详细讨论这个问题。

请随意尝试使用一些其他组件。关于每个[核心组件的文档请参阅此处](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html)。关于[页面创作的详细视频系列请参阅此处](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/aem-sites-authoring-overview.html)。

## 发布更新 {#publish-updates}

AEM 环境分为&#x200B;**作者服务**&#x200B;和&#x200B;**发布服务**。在本章中，我们对&#x200B;**作者服务**&#x200B;上的网站做了一些更改。为了让网站访问者能够看到这些更改，我们需要将更改发布到&#x200B;**发布服务**。

![高级示意图](assets/author-content-publish/author-publish-high-level-flow.png)

*从作者到发布的高级示意内容流程图*

**1.** 内容作者更新网站内容。这些更新可以被预览、查看并被批准推送上线。

**2.** 发布内容。可以按需发布，也可以计划在未来的某个日期发布。

**3.** 网站访问者将看到发布服务上反映的变化。

### 发布更改

接下来，我们来发布更改。

1. 从 AEM 开始屏幕导航至&#x200B;**网站**，然后选择 **WKND 网站**。
1. 在菜单栏中点击&#x200B;**管理发布**。

   ![管理发布](assets/author-content-publish/click-manage-publiciation.png)

   由于这是一个全新的网站，我们希望发布所有页面，可以使用管理发布向导来准确定义需要发布的内容。

1. 在&#x200B;**选项**&#x200B;中保留&#x200B;**发布**&#x200B;默认设置，计划&#x200B;**现在**&#x200B;发布。单击&#x200B;**下一步**。
1. 在&#x200B;**范围**&#x200B;中选择 **WKND 网站**，然后点击&#x200B;**包括子设置**。在对话框中勾选&#x200B;**包括子项**。取消勾选其余的复选框，确保发布整个网站。

   ![更新发布范围](assets/author-content-publish/update-scope-publish.png)

1. 点击&#x200B;**已发布引用**&#x200B;按钮。在对话框中，验证已勾选所有内容。这包括&#x200B;**标准网站模板**&#x200B;以及网站模板生成的一些配置。点击&#x200B;**完成**，进行更新。

   ![发布引用](assets/author-content-publish/publish-references.png)

1. 最后，勾选 **WKND 网站**&#x200B;旁边的复选框，然后点击右上角的&#x200B;**下一个**。
1. 在&#x200B;**工作流**&#x200B;步骤，输入一个&#x200B;**工作流标题**。这可以是任何文本，以后可能用作审核记录的一部分。输入“初次发布”，然后点击&#x200B;**发布**。

![工作流步骤初次发布](assets/author-content-publish/workflow-step-publish.png)

## 查看已发布的内容 {#publish}

接下来，导航到发布服务查看更改。

1. 获取发布服务 URL 的一个简单方法是复制作者 URL，然后将 `author` 词替换为 `publish`。例如：

   * **作者 URL** - `https://author-pYYYY-eXXXX.adobeaemcloud.com/`
   * **发布 URL** - `https://publish-pYYYY-eXXXX.adobeaemcloud.com/`

1. 将 `/content/wknd.html` 添加到发布 URL，使最终 URL 如下所示：`https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd.html`。

   >[!NOTE]
   >
   > 如果您在[网站创建](create-site.md)过程中提供了一个唯一名称，应更改 `wknd.html` 以匹配您的网站名称。

1. 导航到发布 URL，您会看到该网站，但没有任何 AEM 创作功能。

   ![已发布网站](assets/author-content-publish/publish-url-update.png)

1. 使用&#x200B;**导航**&#x200B;菜单，点击&#x200B;**文章** > **Hello World**，导航到之前创建的 Hello World 页面。
1. 返回到 **AEM 作者服务**，在页面编辑器中再做一些内容更改。
1. 在页面编辑器中点击&#x200B;**页面属性**&#x200B;图标 > **发布页面**，就可以直接从页面编辑器中发布这些更改

   ![直接发布](assets/author-content-publish/page-editor-publish.png)

1. 返回到&#x200B;**AEM 发布服务**，查看更改。您很可能&#x200B;**不会**&#x200B;立即看到更新。这是因为 **AEM 发布服务**&#x200B;包括[通过 Apache Web 服务器和内容传递网络进行的缓存](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/caching.html)。默认情况下，HTML 内容被缓存约 5 分钟。

1. 如果要绕过缓存进行测试/调试，只需添加一个查询参数，例如 `?nocache=true`。URL 类似这样：`https://publish-pYYYY-eXXXX.adobeaemcloud.com/content/wknd/en/article/hello-world.html?nocache=true`。有关缓存策略和可用配置的更多详细信息[请参阅此处](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/overview.html)。

1. 您还可以在 Cloud Manager 中找到发布服务的 URL。导航到 **Cloud Manager 程序** > **环境** > **环境**。

   ![查看发布服务](assets/author-content-publish/view-environment-segments.png)

   在&#x200B;**环境分区**&#x200B;中，您可以找到&#x200B;**创作**&#x200B;和&#x200B;**发布**&#x200B;服务的链接。

## 恭喜！ {#congratulations}

祝贺您创作并发布了对 AEM 网站的更改！

### 后续步骤 {#next-steps}

在实际实施中，通常是在网站创建之前使用模型和 UI 设计进行网站规划。阅读[使用 Adobe XD 进行 UI 规划](./ui-planning-adobe-xd.md)，了解如何使用 Adobe XD UI 套件来设计和加速您的 Adobe Experience Manager Sites 实施。

您想继续探索 AEM Sites 的功能吗？请直接前往[页面模板](./page-templates.md)一章，了解页面模板与页面之间的关系。


