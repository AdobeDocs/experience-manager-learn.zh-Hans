---
title: AEM Headless教程
description: 有关如何使用Adobe Experience Manager作为无头CMS的教程集合。
feature: 内容片段、 API
topic: 无外设、内容管理
role: Developer
level: Beginner
source-git-commit: db9f4d09dcc83f85c8d02d94c383fa456af88c24
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 4%

---


# AEM Headless教程

Adobe Experience Manager有多个选项可用于定义无头端点并将其内容交付为JSON。 使用动手实践教程了解如何使用各种选项并选择适合您的选项。

## AEM GraphQL API教程

AEM GraphQL API（用于内容片段）
支持无头CMS方案，其中外部客户端应用程序使用AEM中管理的内容渲染体验。

现代内容交付API是基于Javascript的前端应用程序效率和性能的关键。 使用REST API会带来以下挑战：

* 一次获取一个对象的请求数量很大
* 通常是“过度投放”内容，这意味着应用程序收到的内容超出了其需求

为了克服这些难题，GraphQL提供了一个基于查询的API，允许客户仅查询AEM所需的内容，并使用单个API调用接收。

* 在[AEM GraphQL API快速入门教程](./graphql/overview.md)中了解如何使用AEM GraphQL API

## 基于令牌的身份验证教程

AEM公开了各种可以无头方式交互的HTTP端点，从GraphQL、AEM内容服务到资产HTTP API。 通常，这些无头用户可能需要对AEM进行身份验证才能访问受保护的内容或操作。 为了实现此目的，AEM支持对来自外部应用程序、服务或系统的HTTP请求进行基于令牌的身份验证。

* 了解如何使用[从外部应用程序Cloud Service对AEM进行身份验证教程](./authentication/overview.md)中的访问令牌，通过HTTP对AEM进行身份验证

## AEM Content Services教程

AEM内容服务利用传统的AEM页面来撰写无头REST API端点，并且AEM组件定义或引用要在这些端点上公开的内容。

AEM Content Services允许使用与在AEM Sites中创作网页相同的内容抽象概念，来定义这些HTTP API的内容和模式。 使用AEM页面和AEM组件，营销人员能够快速编写和更新可支持任何应用程序的灵活JSON API。

* 在[AEM Content Services快速入门教程](./content-services/overview.md)中了解如何使用AEM Content Services

## AEM GraphQL与AEM Content Services

|  | AEM GraphQL API | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| 架构定义 | 结构化内容片段模型 | AEM组件 |
| 内容 | 内容片段 | AEM组件 |
| 内容发现 | 按GraphQL查询 | 按AEM页面 |
| 投放格式 | GraphQL JSON | AEM ComponentExporter JSON |

## 其他有用教程

与无头概念相关的其他AEM教程包括：

* [AEM SPA Editor 和 Angular 快速入门](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
* [AEM SPA Editor 和 React 快速入门](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)