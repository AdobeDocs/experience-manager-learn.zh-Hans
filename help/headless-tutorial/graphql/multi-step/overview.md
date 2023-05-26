---
title: AEM Headless实践教程快速入门 — GraphQL
description: 一个端到端教程，其中演示了如何使用AEM GraphQL API构建和公开内容。
doc-type: tutorial
mini-toc-levels: 1
kt: 6678
thumbnail: KT-6678.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-10-17T00:00:00Z
exl-id: 41e15a2c-758b-4e7d-9d23-ef671c1dc155
source-git-commit: 4c0770eafbbbb90bfc00ab49be02e84c41d63057
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 4%

---

# AEM Headless 快速入门 – GraphQL

{{aem-headless-trials-promo}}

一个端到端教程，说明如何在Headless CMS场景中使用AEM GraphQL API构建和公开内容并由外部应用程序使用。

本教程探讨了如何使用AEM GraphQL API和Headless功能增强在外部应用程序中浮现的体验。

本教程涵盖以下主题：

* 创建项目配置
* 创建内容片段模型以建模数据
* 根据之前创建的模型创建内容片段。
* 探索如何使用集成的GraphiQL开发工具查询AEM中的内容片段。
* 将GraphQL查询存储或保留到AEM
* 使用示例React应用程序中的持久GraphQL查询

## 前提条件 {#prerequisites}

遵循本教程需要完成以下操作：

* 基本HTML和JavaScript技能
* 必须本地安装以下工具：
   * [Node.js v18](https://nodejs.org/)
   * [Git](https://git-scm.com/)
   * IDE(例如， [Microsoft® Visual Studio Code](https://code.visualstudio.com/))

### AEM环境

要完成本教程，建议使用AEM管理员访问AEMas a Cloud Service环境。 如果您无权访问AEMas a Cloud Service环境，则可以使用 [本地AEMas a Cloud Service快速入门SDK](/help/cloud-service/local-development-environment/aem-runtime.md). 但是，请务必注意，某些产品UI屏幕（例如内容片段导航）是不同的。

## 让我们开始吧！

开始教程，使用 [定义内容片段模型](content-fragment-models.md).

## GitHub项目

源代码和内容包位于 [AEM指南 — WKND GraphQL GitHub项目](https://github.com/adobe/aem-guides-wknd-graphql).

如果您发现教程或代码有问题，请保留 [GitHub问题](https://github.com/adobe/aem-guides-wknd-graphql/issues).
