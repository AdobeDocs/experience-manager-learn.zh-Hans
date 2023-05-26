---
title: AEM Headless第一个教程
description: 了解如何成为AEM Headless的第一个应用程序。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Development
role: Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-16T00:00:00Z
jira: KT-13270
thumbnail: KT-13270.jpeg
source-git-commit: 4c0770eafbbbb90bfc00ab49be02e84c41d63057
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 2%

---


# AEM Headless第一个教程

{{aem-headless-trials-promo}}

欢迎阅读有关使用React构建完全由AEM Headless API和GraphQL提供支持的网页体验的教程。 在本教程中，我们将指导您通过将React、Adobe Experience Manager (AEM) Headless API和GraphQL的强大功能结合使用，来创建动态和交互式Web应用程序。

React是用于构建用户界面的常用JavaScript库，以其简单性、可重用性和基于组件的架构而闻名。 AEM提供了强大的内容管理功能并公开了Headless API，允许开发人员通过各种渠道和应用程序访问存储在AEM中的内容和数据。

利用AEM Headless API，您可以从AEM实例中检索内容、资源和数据，并使用它们来增强React应用程序。 GraphQL是一种灵活的API查询语言，它提供了一种从AEM实例请求特定数据的高效精确方法，从而实现React与AEM之间的无缝集成。

![AEM Headless第一个教程](./assets/overview/overview.png)

在本教程中，我们将指导您逐步完成使用React和AEM Headless API与GraphQL构建Web体验的过程。 您将了解如何设置开发环境、在React和AEM之间建立连接、使用GraphQL查询检索内容以及在Web应用程序中动态呈现内容。

我们将介绍一些主题，例如配置React项目、使用AEM建立身份验证、使用GraphQL从AEM查询内容、处理React组件中的数据，以及通过利用缓存和分页优化性能。

在本教程结束时，您将充分了解如何利用React、AEM Headless API和GraphQL构建强大而富有吸引力的Web体验。 那么，让我们深入了解并开始构建您的下一个Web应用程序！

## 前提条件

### 技能

+ 精通React
+ 熟练掌握GraphQL
+ AEMas a Cloud Service基础知识

### AEM as a Cloud Service

本教程要求拥有AEMas a Cloud Service环境的管理员访问权限。

### 软件

+ [Node.js v16+](https://nodejs.org/en/)
   + 通过运行检查节点版本 `node -v` 从命令行
+ [npm 6+](https://www.npmjs.com/)
   + 通过运行检查npm版本 `npm -v` 从命令行
+ [Git](https://git-scm.com/)
   + 通过运行检查您的Git版本 `git -v` 从命令行

使用 [节点版本管理器(nvm)](https://github.com/nvm-sh/nvm) 寻址在同一台计算机上有多个node.js版本的问题。

确保您有权在计算机上全局安装软件。

## 下一步

现在您的环境已设置完毕，让我们继续下一步骤： [在AEMas a Cloud Service中设置并创作内容](./1-content-modeling.md)
