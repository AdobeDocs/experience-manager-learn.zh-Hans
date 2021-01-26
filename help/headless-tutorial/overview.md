---
title: AEM无头教程
description: 关于如何将Adobe Experience Manager用作无外设CMS的教程集。
translation-type: tm+mt
source-git-commit: eabd8650886fa78d9d177f3c588374a443ac1ad6
workflow-type: tm+mt
source-wordcount: '460'
ht-degree: 4%

---


# AEM无头教程

Adobe Experience Manager有多种选项，用于定义无头端点并将其内容作为JSON提供。 使用实践教程了解如何使用各种选项并选择适合您的选项。

## AEM GraphQL API教程

>[!CAUTION]
>
> AEM GraphQL API for Content Fragments投放可应请求提供。
> 请联系Adobe支持，为AEM启用API作为Cloud Service项目。

AEM GraphQL API for Content Fragments
支持无外设CMS方案，其中外部客户端应用程序使用AEM中管理的内容呈现体验。

现代的内容投放API是基于Javascript前端应用程序的效率和性能的关键。 使用REST API将带来挑战：

* 一次获取一个对象的大量请求
* 通常是“过量交付”内容，这意味着应用程序收到的内容超出其需要

为了克服这些难题，GraphQL提供了一个基于查询的API，允许客户仅查询AEM所需的内容，并通过单个API调用接收。

* 在《AEM GraphQL API快速入门》教程](./graphql/overview.md)中了解如何使用AEM GraphQL API[

## 基于令牌的身份验证教程

AEM公开了各种可以无头方式交互的HTTP端点，从GraphQL、AEM Content Services到Assets HTTP API。 通常，这些无头用户可能需要向AEM进行身份验证才能访问受保护的内容或操作。 为便于实现此目的，AEM支持基于令牌的对来自外部应用程序、服务或系统的HTTP请求的身份验证。

* 了解如何使用[从外部应用程序教程](./authentication/overview.md)将AEM验证为Cloud Service的访问令牌，通过HTTP验证到AEM

## AEM内容服务教程

AEM Content Services利用传统的AEM页面组成无头REST API端点，AEM组件定义或引用要在这些端点上公开的内容。

AEM Content Services允许使用与在AEM Sites创作网页时相同的内容抽象来定义这些HTTP API的内容和模式。 AEM页面和AEM组件的使用使营销人员能快速构建和更新灵活的JSON API，这些JSON API可以支持任何应用程序。

* 在《AEM内容服务入门》教程[中了解如何使用AEM Content Services](./content-services/overview.md)

## AEM GraphQL与AEM Content Services

|  | AEM GraphQL API | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| 模式定义 | 结构化内容片段模型 | AEM组件 |
| 内容 | 内容片段 | AEM组件 |
| 内容发现 | 按GraphQL查询 | 按AEM页 |
| 投放格式 | GraphQL JSON | AEM ComponentExporter JSON |

## 其他实用教程

其他与无头概念相关的AEM教程包括：

* [AEM SPA Editor 和 Angular 快速入门](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
* [AEM SPA Editor 和 React 快速入门](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)