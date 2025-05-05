---
title: 定义内容片段模型 — AEM Headless入门 — GraphQL
description: Adobe Experience Manager (AEM)和GraphQL快速入门。 了解如何在AEM中使用“内容片段模型”对内容进行建模并构建架构。 查看现有模型并创建模型。 了解可用于定义架构的不同数据类型。
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
jira: KT-6712
thumbnail: 22452.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 9400d9f2-f828-4180-95a7-2ac7b74cd3c9
duration: 228
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1110'
ht-degree: 2%

---

# 定义内容片段模型 {#content-fragment-models}

在本章中，了解如何使用&#x200B;**内容片段模型**&#x200B;对内容建模并构建架构。 了解可用于将架构定义为模型一部分的不同数据类型。

我们创建了两个简单模型，**团队**&#x200B;和&#x200B;**人员**。 **团队**&#x200B;数据模型具有名称、简短名称和描述，并引用&#x200B;**人员**&#x200B;数据模型，该模型具有全名、个人资料详细信息、个人资料图片和职业列表。

此外，还欢迎您按照基本步骤创建自己的模型，并调整相应的步骤，如GraphQL查询和React应用程序代码，或只是按照这些章节中概述的步骤进行操作。

## 先决条件 {#prerequisites}

这是一个多部分教程，并假定有[AEM创作环境可用](./overview.md#prerequisites)。

## 目标 {#objectives}

* 创建内容片段模型。
* 确定用于构建模型的可用数据类型和验证选项。
* 了解内容片段模型如何定义&#x200B;**两者**&#x200B;内容片段的数据架构和创作模板。

## 创建项目配置

项目配置包含与特定项目关联的所有内容片段模型，并提供组织模型的方法。 在创建内容片段模型&#x200B;**之前，必须**&#x200B;至少创建一个项目。

1. 登录到AEM **创作**&#x200B;环境（例如`https://author-pYYYY-eXXXX.adobeaemcloud.com/`）
1. 从AEM开始屏幕中，导航到&#x200B;**工具** > **常规** > **配置浏览器**。

   ![导航到配置浏览器](assets/content-fragment-models/navigate-config-browser.png)
1. 单击右上角的&#x200B;**创建**
1. 在生成的对话框中，输入：

   * 标题*：**我的项目**
   * 名称*：**my-project**(偏好使用所有小写字母并使用连字符分隔单词。 此字符串会影响客户端应用程序执行请求的唯一GraphQL端点。)
   * 检查&#x200B;**内容片段模型**
   * 检查&#x200B;**GraphQL持久查询**

   ![我的项目配置](assets/content-fragment-models/my-project-configuration.png)

## 创建内容片段模型

接下来，为&#x200B;**团队**&#x200B;和&#x200B;**人员**&#x200B;创建两个模型。

### 创建人员模型

创建&#x200B;**人员**&#x200B;的模型，该模型是表示属于某个团队的人员的数据模型。

1. 从AEM开始屏幕中，导航到&#x200B;**工具** > **常规** > **内容片段模型**。

   ![导航到内容片段模型](assets/content-fragment-models/navigate-cf-models.png)

1. 导航到&#x200B;**我的项目**&#x200B;文件夹。
1. 点按右上角的&#x200B;**创建**&#x200B;以显示&#x200B;**创建模型**&#x200B;向导。
1. 在&#x200B;**模型标题**&#x200B;字段中，输入&#x200B;**人员**&#x200B;并点按&#x200B;**创建**。 在生成的对话框中，点按&#x200B;**打开**&#x200B;以构建模型。

1. 将&#x200B;**单行文本**&#x200B;元素拖放到主面板上。 在&#x200B;**属性**&#x200B;选项卡上输入以下属性：

   * **字段标签**：**全名**
   * **属性名称**： `fullName`
   * 检查&#x200B;**必需**

   ![全名属性字段](assets/content-fragment-models/full-name-property-field.png)

   **属性名称**&#x200B;定义保留到AEM的属性的名称。 **属性名称**&#x200B;还将此属性的&#x200B;**键**&#x200B;名称定义为数据架构的一部分。 此&#x200B;**键**&#x200B;在通过GraphQL API公开内容片段数据时使用。

1. 点按&#x200B;**数据类型**&#x200B;选项卡，并将&#x200B;**多行文本**&#x200B;字段拖放到&#x200B;**全名**&#x200B;字段下。 输入以下属性：

   * **字段标签**：**个人简历**
   * **属性名称**： `biographyText`
   * **默认类型**：**富文本**

1. 单击&#x200B;**数据类型**&#x200B;选项卡并拖放&#x200B;**内容引用**&#x200B;字段。 输入以下属性：

   * **字段标签**：**个人资料图片**
   * **属性名称**： `profilePicture`
   * **根路径**： `/content/dam`

   配置&#x200B;**根路径**&#x200B;时，您可以单击&#x200B;**文件夹**&#x200B;图标以调出模式窗口来选择路径。 这会限制作者可以使用哪些文件夹填充路径。 `/content/dam`是存储所有AEM Assets（图像、视频、其他内容片段）的根。

1. 向&#x200B;**图片引用**&#x200B;添加验证，以便只有&#x200B;**图像**&#x200B;的内容类型可用于填充该字段。

   ![限制到图像](assets/content-fragment-models/picture-reference-content-types.png)

1. 单击&#x200B;**数据类型**&#x200B;选项卡，并将&#x200B;**枚举**&#x200B;数据类型拖放到&#x200B;**图片引用**&#x200B;字段下。 输入以下属性：

   * **呈现为**：**复选框**
   * **字段标签**：**职业**
   * **属性名称**： `occupation`

1. 使用&#x200B;**添加选项**&#x200B;按钮添加多个&#x200B;**选项**。 对&#x200B;**选项标签**&#x200B;和&#x200B;**选项值**&#x200B;使用相同的值：

   **艺人**，**影响者**，**摄影师**，**旅行者**，**作者**，**YouTuber**

1. 最终&#x200B;**人员**&#x200B;模型应如下所示：

   ![最终人员模型](assets/content-fragment-models/final-author-model.png)

1. 点击&#x200B;**保存**&#x200B;即可保存更改。

### 创建团队模型

为&#x200B;**团队**&#x200B;创建模型，这是人员团队的数据模型。 团队模型引用“人员”模型来表示团队成员。

1. 在&#x200B;**我的项目**&#x200B;文件夹中，点按右上角的&#x200B;**创建**&#x200B;以显示&#x200B;**创建模型**&#x200B;向导。
1. 在&#x200B;**模型标题**&#x200B;字段中，输入&#x200B;**团队**&#x200B;并点按&#x200B;**创建**。

   在生成的对话框中点按&#x200B;**打开**&#x200B;以打开新创建的模型。

1. 将&#x200B;**单行文本**&#x200B;元素拖放到主面板上。 在&#x200B;**属性**&#x200B;选项卡上输入以下属性：

   * **字段标签**：**标题**
   * **属性名称**： `title`
   * 检查&#x200B;**必需**

1. 点按&#x200B;**数据类型**&#x200B;选项卡，并将&#x200B;**单行文本**&#x200B;元素拖放到主面板上。 在&#x200B;**属性**&#x200B;选项卡上输入以下属性：

   * **字段标签**：**短名称**
   * **属性名称**： `shortName`
   * 检查&#x200B;**必需**
   * 检查&#x200B;**唯一**
   * 在下，**验证类型** >选择&#x200B;**自定义**
   * 在下，**自定义验证正则表达式** >输入`^[a-z0-9\-_]{5,40}$` — 这可以确保只能输入5到40个字符之间的小写字母数字值和破折号。

   `shortName`属性提供了一种根据缩短路径查询单个团队的方法。 **唯一**&#x200B;设置确保此模型的每个内容片段的值始终是唯一的。

1. 点按&#x200B;**数据类型**&#x200B;选项卡，并将&#x200B;**多行文本**&#x200B;字段拖放到&#x200B;**短名称**&#x200B;字段下。 输入以下属性：

   * **字段标签**：**描述**
   * **属性名称**： `description`
   * **默认类型**：**富文本**

1. 单击&#x200B;**数据类型**&#x200B;选项卡并拖放&#x200B;**片段引用**&#x200B;字段。 输入以下属性：

   * **呈现为**：**多个字段**
   * **字段标签**：**团队成员**
   * **属性名称**： `teamMembers`
   * **允许的内容片段模型**：使用文件夹图标选择&#x200B;**人员**&#x200B;模型。

1. 最终&#x200B;**团队**&#x200B;模型应如下所示：

   ![最终团队模型](assets/content-fragment-models/final-team-model.png)

1. 点击&#x200B;**保存**&#x200B;即可保存更改。

1. 您现在应该可以从以下两个模型工作：

   ![两个模型](assets/content-fragment-models/two-new-models.png)

## 发布项目配置和内容片段模型

审核和验证后，发布`Project Configuration`和`Content Fragment Model`

1. 从AEM开始屏幕中，导航到&#x200B;**工具** > **常规** > **配置浏览器**。

1. 点按&#x200B;**我的项目**&#x200B;旁边的复选框，然后点按&#x200B;**发布**

   ![发布项目配置](assets/content-fragment-models/publish-project-config.png)

1. 从AEM开始屏幕中，导航到&#x200B;**工具** > **常规** > **内容片段模型**。

1. 导航到&#x200B;**我的项目**&#x200B;文件夹。

1. 点按&#x200B;**人员**&#x200B;和&#x200B;**团队**&#x200B;模型，然后点按&#x200B;**发布**

   ![发布内容片段模型](assets/content-fragment-models/publish-content-fragment-model.png)

## 恭喜！ {#congratulations}

恭喜，您刚刚创建了您的第一个内容片段模型！

## 后续步骤 {#next-steps}

在下一章[创作内容片段模型](author-content-fragments.md)中，您将基于内容片段模型创建和编辑新的内容片段。 您还将了解如何创建内容片段的变体。

## 相关文档

* [内容片段模型](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html?lang=zh-Hans)

