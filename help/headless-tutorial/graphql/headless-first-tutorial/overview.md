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
duration: 89
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 2%

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

## 先决条件

### 技能

+ 熟练掌握React
+ 熟练掌握GraphQL
+ AEM as a Cloud Service基础知识

### AEM as a Cloud Service

本教程需要拥有AEM as a Cloud Service环境的管理员访问权限。

### 软件

+ [Node.js v16+](https://nodejs.org/en/)
   + 通过从命令行运行`node -v`检查节点版本
+ [npm 6+](https://www.npmjs.com/)
   + 通过从命令行运行`npm -v`检查您的npm版本
+ [Git](https://git-scm.com/)
   + 通过从命令行运行`git -v`检查您的Git版本

使用[节点版本管理器(nvm)](https://github.com/nvm-sh/nvm)解决同一计算机上有多个node.js版本的问题。

确保您具有在计算机上全局安装软件的权限。

## 下一步

现在您的环境已设置，让我们进入下一步：[在AEM as a Cloud Service中设置并创作内容](./1-content-modeling.md)
