---
title: AEM Headless无头动手操作教程 — GraphQL
description: 一个端到端教程，其中演示了如何使用AEM GraphQL API构建和公开内容。
doc-type: tutorial
mini-toc-levels: 1
kt: 6678
thumbnail: 328618.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 41e15a2c-758b-4e7d-9d23-ef671c1dc155
source-git-commit: 25c289b093297e870c52028a759d05628d77f634
workflow-type: tm+mt
source-wordcount: '292'
ht-degree: 1%

---

# AEM无头入门 — GraphQL

一个端到端教程，其中演示了如何在无头CMS场景中使用AEM GraphQL API构建和显示由外部应用程序使用的内容。

本教程探讨如何使用AEM GraphQL API和无头功能来为外部应用程序中显示的体验提供动力。

本教程涵盖以下主题：

* 创建项目配置
* 创建内容片段模型以对数据进行建模
* 根据先前制作的模型创建内容片段。
* 了解如何使用集成的GraphiQL开发工具查询AEM中的内容片段。
* 要存储GraphQL查询或将其保留到AEM
* 从示例React应用程序中使用持久GraphQL查询


## 前提条件 {#prerequisites}

以下内容需要阅读本教程：

* 基本HTML和JavaScript技能
* 必须在本地安装以下工具：
   * [Node.js v14+](https://nodejs.org/en/)
   * [npm 6+](https://www.npmjs.com/)
   * [Git](https://git-scm.com/)
   * IDE(例如， [Microsoft® Visual Studio代码](https://code.visualstudio.com/))

### AEM Environment

要完成本教程，建议AEM管理员访问AEMas a Cloud Service环境。 如果您无权访问AEMas a Cloud Service环境，则可以使用 [本地AEMas a Cloud Service快速启动SDK](/help/cloud-service/local-development-environment/aem-runtime.md). 但是，请务必注意，某些产品UI屏幕（如内容片段导航）不同。

## 开始吧！

以开始教程 [定义内容片段模型](content-fragment-models.md).

## GitHub项目

源代码和内容包位于 [AEM指南 — WKND GraphQL GitHub项目](https://github.com/adobe/aem-guides-wknd-graphql).

如果您发现本教程或代码存在问题，请保留 [GitHub问题](https://github.com/adobe/aem-guides-wknd-graphql/issues).
