---
title: AEM Headless入门指南实践教程 - GraphQL
description: 一个端到端教程，它演示了如何使用 AEM GraphQL API 生成和展示内容。
doc-type: Tutorial
mini-toc-levels: 1
jira: KT-6678
thumbnail: KT-6678.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-10-17T00:00:00Z
exl-id: 41e15a2c-758b-4e7d-9d23-ef671c1dc155
duration: 54
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: ht
source-wordcount: '237'
ht-degree: 100%

---

# AEM Headless 快速入门 – GraphQL

这是一份端到端的教程，详细介绍了如何在 Headless CMS 场景下，利用 AEM 的 GraphQL API 构建和公开内容，并供外部应用程序使用。

本教程将探讨如何利用 AEM 的 GraphQL API 和 Headless 功能，为外部应用程序中呈现的体验提供支持。

本教程涵盖以下主题：

* 创建项目配置
* 创建内容片段模型以对数据进行建模
* 根据之前建立的模型创建内容片段。
* 探索如何使用集成的 GraphiQL 开发工具查询 AEM 中的内容片段。
* 将 GraphQL 查询存储或持久化到 AEM 中
* 从示例 React 应用程序中消费持久化的 GraphQL 查询

## 先决条件 {#prerequisites}

学习本教程需具备以下条件：

* 基本的 HTML 和 JavaScript 技能
* 必须在本地安装以下工具：
   * [Node.js v18](https://nodejs.org/)
   * [Git](https://git-scm.com/)
   * IDE（例如，[Microsoft® Visual Studio Code](https://code.visualstudio.com/)）

### AEM 环境

要完成本教程，建议您具有 AEM as a Cloud Service 环境的 AEM 管理员访问权限。

## 让我们开始吧！

从[定义内容片段模型](content-fragment-models.md)开始学习本教程。

## GitHub 项目

本教程所用的源代码和内容包可在 [AEM Guides - WKND GraphQL GitHub 项目](https://github.com/adobe/aem-guides-wknd-graphql)中获取。

如果您在使用教程或代码时发现问题，欢迎[在 GitHub 提交问题](https://github.com/adobe/aem-guides-wknd-graphql/issues)。
