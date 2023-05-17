---
title: AEM Headless第一个教程
description: 了解如何成为AEM Headless第一个应用程序。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
source-git-commit: 12b3888552d5a131628dabf380840f0586798ea5
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 2%

---


# AEM Headless第一个教程

![AEM Headless第一个教程](./assets/overview/overview.png)

欢迎参加有关使用由AEM Headless API和GraphQL完全支持的React构建Web体验的教程。 在本教程中，我们将指导您完成通过结合React、Adobe Experience Manager(AEM)无头API和GraphQL的强大功能来创建动态交互式Web应用程序的过程。

React是一款用于构建用户界面的常用JavaScript库，以其简单性、可重用性和基于组件的架构而闻名。 AEM提供了强大的内容管理功能，并公开了无头API，使开发人员能够通过各种渠道和应用程序访问AEM中存储的内容和数据。

利用AEM无头API，您可以从AEM实例中检索内容、资产和数据，并使用它们为React应用程序提供动力。 GraphQL是一种针对API的灵活查询语言，它提供了一种高效、精确的方式来从AEM实例请求特定数据，从而实现了React和AEM之间的无缝集成。

在本教程中，我们将指导您逐步完成使用React和AEM Headless API与GraphQL构建Web体验的过程。 您将学习如何设置开发环境、在React和AEM之间建立连接、使用GraphQL查询检索内容，以及在Web应用程序中动态渲染内容。

我们将介绍以下主题：配置React项目、使用AEM建立身份验证、使用GraphQL从AEM查询内容、处理React组件中的数据，以及通过缓存和分页优化性能。

在本教程结束时，您将充分了解如何利用React、AEM Headless API和GraphQL构建强大而引人入胜的Web体验。 因此，让我们深入研究并开始构建您的下一个Web应用程序！

## 前提条件

### 技能

+ 反应能力
+ 熟练掌握GraphQL
+ AEMas a Cloud Service基础知识

### AEM as a Cloud Service

本教程需要管理员对AEMas a Cloud Service环境的访问权限。

### 软件

+ [Node.js v16+](https://nodejs.org/en/)
   + 通过运行 `node -v` 从命令行
+ [npm 6+](https://www.npmjs.com/)
   + 通过运行检查npm版本 `npm -v` 从命令行
+ [Git](https://git-scm.com/)
   + 通过运行 `git -v` 从命令行

使用 [节点版本管理器(nvm)](https://github.com/nvm-sh/nvm) 来解决在同一台计算机上具有多个版本的node.js的问题。

确保您有权在计算机上全局安装软件。

## 下一步

现在，您的环境已设置完成，接下来让我们继续执行下一步： [在AEMas a Cloud Service中设置和创作内容](./1-content-modeling.md)
