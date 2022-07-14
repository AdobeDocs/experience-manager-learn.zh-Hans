---
title: 定义内容片段模型 — AEM无头入门 — GraphQL
description: 开始使用Adobe Experience Manager(AEM)和GraphQL。 了解如何在AEM中使用内容片段模型来建模内容和构建模式。 查看现有模型并创建新模型。 了解可用于定义模式的不同数据类型。
version: Cloud Service
mini-toc-levels: 1
kt: 6712
thumbnail: 22452.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 9400d9f2-f828-4180-95a7-2ac7b74cd3c9
source-git-commit: a49e56b6f47e477132a9eee128e62fe5a415b262
workflow-type: tm+mt
source-wordcount: '1223'
ht-degree: 2%

---

# 定义内容片段模型 {#content-fragment-models}

在本章中，了解如何对内容建模并使用 **内容片段模型**. 您将了解可用于定义模式作为模型一部分的不同数据类型。

本章将创建两个简单的模型， **团队** 和 **人员**. 的 **团队** 数据模型具有名称、短名称和描述，并引用 **人员** 数据模型，包含全名、个人简介、个人简介和职业列表。

此外，您还欢迎按照基本步骤创建自己的模型，并调整相应的步骤（如GraphQL查询和React应用程序代码），或者只是按照这些章节中概述的步骤操作。

## 前提条件 {#prerequisites}

这是一个多部分教程，假定 [AEM创作环境可用](./overview.md#prerequisites) （可选） [已安装WKND共享示例内容](./overview.md#install-sample-content).

## 目标 {#objectives}

* 创建新的内容片段模型。
* 确定用于构建模型的可用数据类型和验证选项。
* 了解内容片段模型如何定义 **both** 内容片段的数据架构和创作模板。

## 创建新项目配置

项目配置包含与特定项目关联的所有内容片段模型，并提供了组织模型的方法。 必须至少创建一个项目 **之前** 创建新的内容片段模型。

1. 登录AEM **作者** 环境。
1. 从AEM开始屏幕中，导航到 **工具** > **常规** > **配置浏览器**.

   ![导航到配置浏览器](assets/content-fragment-models/navigate-config-browser.png)
1. 单击&#x200B;**创建**。
1. 在结果对话框中，输入：

   * 标题*: **我的项目**
   * 名称*: **my-project** (首选使用连字符使用全部小写来分隔单词。 此字符串将影响客户端应用程序将针对其执行请求的唯一GraphQL端点。)
   * 检查 **内容片段模型**
   * 检查 **GraphQL永久查询**

   ![我的项目配置](assets/content-fragment-models/my-project-configuration.png)

## 创建内容片段模型

接下来，为 **团队** 和 **人员**.

### 创建人员模型

为 **人员**，表示属于团队的人员的数据模型。

1. 从AEM开始屏幕中，导航到 **工具** > **常规** > **内容片段模型**.

   ![导航到内容片段模型](assets/content-fragment-models/navigate-cf-models.png)

   如果您安装了 [示例内容](overview.md#install-sample-content) 然后，您将看到两个文件夹： **我的项目** 和 **WKND共享**.
1. 导航到 **我的项目** 文件夹。
1. 点按 **创建** 在右上角， **创建模型** 向导。
1. 对于 **模型标题** 输入： **人员** 点按 **创建**.

   点按 **打开** 在结果对话框中，打开新创建的模型。

1. 拖放 **单行文本** 元素。 在 **属性** 选项卡：

   * **字段标签**: **全名**
   * **属性名称**: `fullName`
   * 检查 **必需**

   ![“全名”属性字段](assets/content-fragment-models/full-name-property-field.png)

   的 **属性名称** 定义保留到AEM的属性的名称。 的 **属性名称** 还定义 **key** 作为数据架构一部分的此属性的名称。 此 **key** 将在通过GraphQL API公开内容片段数据时使用。

1. 点按 **数据类型** 选项卡，并拖放 **多行文本** 字段 **全名** 字段。 输入以下属性：

   * **字段标签**: **传记**
   * **属性名称**: `biographyText`
   * **默认类型**: **富文本**

1. 单击 **数据类型** 选项卡，并拖放 **内容参考** 字段。 输入以下属性：

   * **字段标签**: **配置文件图片**
   * **属性名称**: `profilePicture`
   * **根路径**: `/content/dam`

   配置 **根路径** 您可以单击 **文件夹** 图标来显示用于选择路径的模式窗口。 这将限制作者可以使用哪些文件夹来填充路径。 `/content/dam` 是存储所有AEM资产（图像、视频、其他内容片段）的根。

1. 向 **图片参考** 以便仅查看内容类型 **图像** 可用于填充字段。

   ![限制为图像](assets/content-fragment-models/picture-reference-content-types.png)

1. 单击 **数据类型** 选项卡，并拖放 **明细列表**  下方的数据类型 **图片参考** 字段。 输入以下属性：

   * **渲染为**: **复选框**
   * **字段标签**: **职业**
   * **属性名称**: `occupation`

1. 添加多个 **选项** 使用 **添加选项** 按钮。 对 **选项标签** 和 **选项值**:

   **艺术家**, **影响者**, **摄影师**, **旅行者**, **作者**, **YouTube**

1. 最后 **人员** 模型应如下所示：

   ![最终人员模型](assets/content-fragment-models/final-author-model.png)

1. 单击 **保存** 以保存更改。

### 创建团队模型

为 **团队**，这是一组人员的数据模型。 “团队”模型将引用“人员”模型来表示团队的成员。

1. 在 **我的项目** 文件夹，点按 **创建** 在右上角， **创建模型** 向导。
1. 对于 **模型标题** 输入： **团队** 点按 **创建**.

   点按 **打开** 在结果对话框中，打开新创建的模型。

1. 拖放 **单行文本** 元素。 在 **属性** 选项卡：

   * **字段标签**: **标题**
   * **属性名称**: `title`
   * 检查 **必需**

1. 点按 **数据类型** 选项卡，并拖放 **单行文本** 元素。 在 **属性** 选项卡：

   * **字段标签**: **短名称**
   * **属性名称**: `shortName`
   * 检查 **必需**
   * 检查 **独特**
   * 在 **验证类型** >选择 **自定义**
   * 在 **自定义验证正则表达式** > enter `^[a-z0-9\-_]{5,40}$`  — 这将确保只能输入小写字母数字值和5到40个字符之间的短划线。

   的 `shortName` 属性将为我们提供一种基于缩短路径查询单个团队的方法。 的 **独特** 设置可确保该值始终为此模型中每个内容片段的唯一值。

1. 点按 **数据类型** 选项卡，并拖放 **多行文本** 字段 **短名称** 字段。 输入以下属性：

   * **字段标签**: **描述**
   * **属性名称**: `description`
   * **默认类型**: **富文本**

1. 单击 **数据类型** 选项卡，并拖放 **片段引用** 字段。 输入以下属性：

   * **渲染为**: **多字段**
   * **字段标签**: **团队成员**
   * **属性名称**: `teamMembers`
   * **允许的内容片段模型**:使用文件夹图标选择 **人员** 模型。

1. 最后 **团队** 模型应如下所示：

   ![最终团队模型](assets/content-fragment-models/final-team-model.png)

1. 单击 **保存** 以保存更改。

1. 现在，您应该从以下两个模型开始工作：

   ![两种模型](assets/content-fragment-models/two-new-models.png)

## Inspect WKND内容片段模型（可选）

如果您 [已安装WKND共享示例内容](./overview.md#install-sample-content) 您可以检查冒险、文章和作者模型，以了解有关数据建模技术的更多信息。

1. 从 **AEM开始** 菜单导航到 **工具** > **常规** > **内容片段模型**.

1. 导航到 **WKND共享** 文件夹下，您应该看到三种模型：文章、冒险和作者。

1. Inspect ，方法是将鼠标悬停在卡片上并点按编辑图标（铅笔）

   ![WKND模型](assets/content-fragment-models/wknd-shared-models.png)

1. 这将打开 **内容片段模型编辑器** 对于模型，您可以检查使用的各种数据类型。

   >[!CAUTION]
   >
   > 修改模型 **after** 内容片段已创建，具有下游效果。 现有片段中的字段值将不再被引用，GraphQL公开的数据架构将发生更改，从而影响现有应用程序。

## 恭喜！ {#congratulations}

恭喜，您刚刚创建了您的第一个内容片段模型！

## 后续步骤 {#next-steps}

在下一章中， [创作内容片段模型](author-content-fragments.md)，您将根据内容片段模型创建和编辑新的内容片段。 您还将了解如何创建内容片段的变体。

## 相关文档

* [内容片段模型](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/content-fragments/content-fragments-models.html)

