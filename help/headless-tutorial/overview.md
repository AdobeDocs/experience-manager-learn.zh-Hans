---
title: AEM无头教程
description: 有关如何将Adobe Experience Manager用作无外设CMS的教程集。
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '460'
ht-degree: 4%

---


# AEM无头教程

Adobe Experience Manager有多种选项，用于定义无外设端点并将其内容作为JSON提供。 使用实践教程探索如何使用各种选项并选择适合您的选项。

## AEM GraphQL API教程

>[!CAUTION]
>
> AEM GraphQL API for Content Fragments投放可应请求提供。
> 请联系Adobe支持，以将AEM的API作为Cloud Service项目。

AEM GraphQL API，用于内容片段
支持无外设CMS方案，外部客户端应用程序使用AEM中管理的内容来呈现体验。

现代内容投放API是基于Javascript前端应用程序的效率和性能的关键。 使用REST API会带来挑战：

* 一次获取一个对象的大量请求
* 通常是“过量交付”内容，这意味着应用程序收到的内容超出其需要

为克服这些难题，GraphQL提供了基于查询的API，使客户能够仅查询AEM所需的内容，并通过单个API调用接收。

* 在[AEM GraphQL API入门教程](./graphql/overview.md)中了解如何使用AEM GraphQL API

## 基于令牌的身份验证教程

AEM公开了各种可以无外设交互的HTTP端点，从GraphQL、AEM Content Services到Assets HTTP API。 通常，这些无外设消费者可能需要向AEM进行身份验证才能访问受保护的内容或操作。 为了便于执行此操作，AEM支持对来自外部应用程序、服务或系统的HTTP请求进行基于令牌的身份验证。

* 了解如何使用[从外部应用程序教程](./authentication/overview.md)中的访问令牌对AEM进行身份验证，以作为Cloud Service，通过HTTP验证到AEM

## AEM内容服务教程

AEM Content Services利用传统的AEM页面构建无头REST API端点，而AEM组件定义或引用要在这些端点上公开的内容。

AEM Content Services允许使用与在AEM Sites中创作网页相同的内容抽象来定义这些HTTP API的内容和模式。 AEM页面和AEM组件的使用使营销人员能快速编写和更新灵活的JSON API，这些JSON API可以支持任何应用程序。

* 阅读《AEM内容服务入门》教程](./content-services/overview.md)，了解如何使用AEM内容服务[

## AEM GraphQL与AEM内容服务

|  | AEM GraphQL API | AEM Content Services |
|--------------------------------|:-----------------|:---------------------|
| 模式定义 | 结构化内容片段模型 | AEM组件 |
| 内容 | 内容片段 | AEM组件 |
| 内容发现 | 按图表QL查询 | 按AEM页 |
| 投放格式 | GraphQL JSON | AEM ComponentExporter JSON |

## 其他有用的教程

其他与无外设概念相关的AEM教程包括：

* [AEM SPA Editor 和 Angular 快速入门](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
* [AEM SPA Editor 和 React 快速入门](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)