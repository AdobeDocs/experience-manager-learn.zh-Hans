---
title: 以 AEM Headless 为核心的教程
description: 了解如何创建以 AEM Headless 为核心的应用程序。
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
exl-id: b0ac4b50-5fe5-41a1-9530-8e593d7000c9
duration: 89
source-git-commit: bd0f42fa37b7bbe19bf0d7fc65801198e64cbcd9
workflow-type: ht
source-wordcount: '421'
ht-degree: 100%

---

# 以 AEM Headless 为核心的教程

欢迎参加本教程，学习如何使用 React 构建由 AEM Headless API 和 GraphQL 全面驱动的 Web 体验。在本教程中，我们将引导您通过结合 React、Adobe Experience Manager (AEM) Headless API 和 GraphQL 的强大功能，创建一个动态且互动的 Web 应用程序。

React 是一个流行的 JavaScript 库，用于构建用户界面，以其简洁性、可重用性和基于组件的架构著称。AEM 提供强大的内容管理功能，并通过 Headless API 向开发人员开放，允许他们通过多种渠道和应用程序访问存储在 AEM 中的内容和数据。

通过利用 AEM Headless API，您可以从 AEM 实例中获取内容、资产和数据，并将其用于驱动您的 React 应用程序。GraphQL 是一种灵活的 API 查询语言，可提供高效且精准的方式来请求 AEM 实例中的特定数据，实现 React 与 AEM 之间的无缝集成。

![以 AEM Headless 为核心的教程](./assets/overview/overview.png)

在本教程中，我们将逐步引导您使用 React 结合 AEM Headless API 和 GraphQL 构建 Web 体验。您将学习如何搭建开发环境、建立 React 与 AEM 的连接、使用 GraphQL 查询检索内容，并在您的 Web 应用中动态呈现这些内容。

我们将涵盖的内容包括配置 React 项目、与 AEM 建立身份验证、使用 GraphQL 查询 AEM 内容、在 React 组件中处理数据，以及通过缓存和分页优化性能。

通过本教程的学习，您将全面掌握如何利用 React、AEM Headless API 和 GraphQL 构建强大且富有吸引力的 Web 体验。那我们开始吧，一起动手构建您的下一个 Web 应用程序！

## 先决条件

### 技能

+ 熟练掌握 React
+ 熟练掌握 GraphQL
+ AEM as a Cloud Service 基础知识

### AEM as a Cloud Service

本教程需要具备对 AEM as a Cloud Service 环境的管理员访问权限。

### 软件

+ [Node.js v16+](https://nodejs.org/en/)
   + 通过命令行运行 `node -v` 来检查您的节点版本
+ [npm 6+](https://www.npmjs.com/)
   + 通过命令行运行 `npm -v` 来检查您的 npm 版本
+ [Git](https://git-scm.com/)
   + 通过命令行运行 `git -v` 来检查您的 git 版本

使用[节点版本管理器 (nvm) ](https://github.com/nvm-sh/nvm)来解决同一台机器上存在多个 Node.js 版本的问题。

请确保您拥有在计算机上全局安装软件的权限。

## 下一步

环境配置完成后，接下来我们进入下一步：[在 AEM as a Cloud Service 中进行内容设置与创作](./1-content-modeling.md)。
