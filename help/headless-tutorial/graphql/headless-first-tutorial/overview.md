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
exl-id: b0ac4b50-5fe5-41a1-9530-8e593d7000c9
duration: 99
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 1%

---

# AEM Headless第一个教程

{{aem-headless-trials-promo}}

欢迎阅读关于使用AEM Headless API和GraphQL完全提供支持的React构建Web体验的教程。 在本教程中，我们将指导您通过将React、Adobe Experience Manager (AEM) Headless API和GraphQL的强大功能结合使用，来创建动态的交互式Web应用程序。

React是用于构建用户界面的常用JavaScript库，以其简单性、可重用性和基于组件的架构而闻名。 AEM提供了强大的内容管理功能并公开了Headless API，允许开发人员通过各种渠道和应用程序访问存储在AEM中的内容和数据。

利用AEM Headless API，您可以从AEM实例中检索内容、资源和数据，并使用它们来增强React应用程序。 GraphQL是一种灵活的API查询语言，可提供一种高效且精确的方法来从AEM实例请求特定数据，从而实现React和AEM之间的无缝集成。

![AEM Headless第一个教程](./assets/overview/overview.png)

在本教程中，我们将指导您逐步完成使用React和AEM Headless API与GraphQL构建Web体验的过程。 您将学习如何设置开发环境、在React和AEM之间建立连接、使用GraphQL查询检索内容以及在Web应用程序中动态呈现内容。

我们将介绍配置React项目、使用AEM建立身份验证、使用GraphQL从AEM查询内容、处理React组件中的数据以及通过利用缓存和分页优化性能等主题。

在本教程结束时，您将充分了解如何利用React、AEM Headless API和GraphQL构建强大而引人入胜的Web体验。 那么，让我们深入了解并开始构建您的下一个Web应用程序！

## 前提条件

### 技能

+ 熟练掌握React
+ 熟练掌握GraphQL
+ AEMas a Cloud Service基础知识

### AEM as a Cloud Service

本教程需要拥有AEMas a Cloud Service环境的管理员访问权限。

### 软件

+ [Node.js v16+](https://nodejs.org/en/)
   + 通过运行检查节点版本 `node -v` 从命令行
+ [npm 6+](https://www.npmjs.com/)
   + 通过运行检查npm版本 `npm -v` 从命令行
+ [Git](https://git-scm.com/)
   + 通过运行检查您的Git版本 `git -v` 从命令行

使用 [节点版本管理器(nvm)](https://github.com/nvm-sh/nvm) 寻址在同一台计算机上拥有多个版本的node.js。

确保您具有在计算机上全局安装软件的权限。

## 下一步

现在您的环境已设置完毕，下面我们来看看下一步： [在AEMas a Cloud Service设置和创作内容](./1-content-modeling.md)
