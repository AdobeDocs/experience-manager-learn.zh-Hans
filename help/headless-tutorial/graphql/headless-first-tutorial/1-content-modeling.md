---
title: 内容建模 — AEM Headless第一个教程
description: 了解如何在AEM中利用内容片段、创建片段模型和使用GraphQL端点。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
exl-id: 6e5e3cb4-9a47-42af-86af-da33fd80cb47
duration: 242
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 9%

---

# 内容建模

欢迎阅读有关Adobe Experience Manager (AEM)中的内容片段和GraphQL端点的教程一章。 我们将介绍如何利用内容片段、创建片段模型以及在AEM中使用GraphQL端点。

内容片段提供了一种用于跨渠道管理内容的结构化方法，提供了灵活性和可重用性。 在AEM中启用内容片段允许创建模块化内容，从而增强一致性和适应性。

首先，我们将引导您在AEM中启用内容片段，其中包括实现无缝集成所需的配置和设置。

接下来，我们将介绍如何创建片段模型，其中定义了结构和属性。 了解如何根据您的内容要求设计模型并有效地管理它们。

然后，我们将演示如何从模型创建内容片段，从而提供有关创作和发布的分步指南。

此外，我们将探讨定义AEM GraphQL端点。 GraphQL会高效地从AEM中检索数据，我们将设置和配置端点以公开所需数据。 持久查询将优化性能和缓存。

在本教程中，我们将提供说明、代码示例和实用提示。 到最后，您将具备启用内容片段、创建片段模型、生成片段以及定义AEM GraphQL端点和持久查询的技能。 让我们开始吧！

## 上下文感知配置

1. 导航到 __“工具”>“配置浏览器”__ 为headless体验创建配置。

   ![创建文件夹](./assets/1/create-configuration.png)

   提供 __标题__ 和 __name__，并选中 __GraphQL持久查询__ 和 __内容片段模型__.


## 内容片段模型

1. 导航到 __工具>内容片段模型__ 并选择具有在步骤1中创建的配置名称的文件夹。

   ![模型文件夹](./assets/1/model-folder.png)

1. 在文件夹中，选择 __创建__ 并命名模型 __Teaser__. 将以下数据类型添加到 __Teaser__ 模型。

   | 数据类型 | 名称 | 必填 | 选项 |
   |----------|------|----------|---------|
   | 内容引用 | 资产 | 是 | 根据需要添加默认图像。 例如： /content/dam/wknd-headless/assets/AdobeStock_307513975.mp4 |
   | 单行文本 | 标题 | 是 |
   | 单行文本 | 前置标题 | 否 |
   | 多行文本 | 描述 | 否 | 确保默认类型为RTF |
   | 枚举 | 样式 | 是 | 呈现为下拉列表。 选项为Hero ->Hero和Featured ->精选 |

   ![Teaser模型](./assets/1/teaser-model.png)

1. 在文件夹内，创建名为的第二个模型 __选件__. 单击“创建”并为模型命名“选件”，然后添加以下数据类型：

   | 数据类型 | 名称 | 必填 | 选项 |
   |----------|------|----------|---------|
   | 内容引用 | 资产 | 是 | 添加默认图像。 例如： `/content/dam/wknd-headless/assets/AdobeStock_238607111.jpeg` |
   | 多行文本 | 描述 | 否 |  |
   | 多行文本 | 文章 | 否 |  |

   ![优惠模式](./assets/1/offer-model.png)

1. 在文件夹内，创建名为的第三个模型 __图像列表__. 单击“创建”并为模型命名“图像列表”，然后添加以下数据类型：

   | 数据类型 | 名称 | 必填 | 选项 |
   |----------|------|----------|---------|
   | 片段引用 | 列表项目 | 是 | 呈现为多个字段。 允许的内容片段模型为“选件”。 |

   ![图像列表模型](./assets/1/imagelist-model.png)

## 内容片段

1. 现在，导航到Assets ，并为新站点创建一个文件夹。 单击创建并命名文件夹。

   ![添加文件夹](./assets/1/create-folder.png)

1. 创建文件夹后，选择该文件夹并打开其 __属性__.
1. 在文件夹的 __云配置__ 选项卡，选择配置 [先前创建](#enable-content-fragments-and-graphql).

   ![资产文件夹AEM Headless云配置](./assets/1/cloud-config.png)

   单击进入新文件夹并创建Teaser。 单击 __创建__ 和 __内容片段__ 并选择 __Teaser__ 模型。 命名模型 __英雄__ 并单击 __创建__.

   | 名称 | 注释 |
   |----------|------|
   | 资产 | 保留为默认值或选择其他资源（视频或图像） |
   | 标题 | `Explore. Discover. Live.` |
   | 前置标题 | `Join use for your next adventure.` |
   | 描述 | 留空 |
   | 样式 | `Hero` |

   ![主页片段](./assets/1/teaser-model.png)

## GraphQL 端点

1. 导航到 __“工具”>“GraphQL”__

   ![AEM Graphiql](./assets/1/endpoint-nav.png)

1. 单击 __创建__ 并为新端点提供一个名称并选择新创建的配置。

   ![AEM Headless GraphQL端点](./assets/1/endpoint.png)

## GraphQL 持久查询

1. 让我们测试新端点。 导航到 __工具> GraphQL查询编辑器__ 并为窗口右上角的下拉列表选择我们的端点。

1. 在查询编辑器中，创建几个不同的查询。


   ```graphql
   {
       teaserList {
           items {
           title
           }
       }
   }
   ```

   您应该获得一个列表，其中包含已创建的单个片段 [以上](#create-content).

   对于本练习，请创建AEM Headless应用程序使用的完整查询。 创建按路径返回单个Teaser的查询。 在查询编辑器中，输入以下查询：

   ```graphql
   query TeaserByPath($path: String!) {
   component: teaserByPath(_path: $path) {
       item {
       __typename
       _path
       _metadata {
           stringMetadata {
           name
           value
           }
       }
       title
       preTitle
       style
       asset {
           ... on MultimediaRef {
           __typename
           _authorUrl
           _publishUrl
           format
           }
           ... on ImageRef {
           __typename
           _authorUrl
           _publishUrl
           mimeType
           width
           height
           }
       }
       description {
           html
           plaintext
       }
       }
   }
   }
   ```

   在 __查询变量__ 在底部输入，输入：

   ```json
   {
       "path": "/content/dam/pure-headless/hero"
   }
   ```

   >[!NOTE]
   >
   > 您可能需要调整查询变量 `path` 基于文件夹和片段名称。


   运行查询以接收之前创建的内容片段的结果。

1. 单击 __保存__  保留（保存）查询并命名查询 __Teaser__. 这允许我们在应用程序中按名称引用查询。

## 后续步骤

恭喜！您已成功将AEMas a Cloud Service配置为允许创建内容片段和GraphQL端点。 您还创建了一个内容片段模型和内容片段，并定义了GraphQL端点和持久查询。 现在，您已准备好进入下一教程章节，您将在该章节中了解如何创建使用您在本章中创建的内容片段和GraphQL端点的AEM Headless React应用程序。

[下一章：AEM Headless API和React](./2-aem-headless-apis-and-react.md)
