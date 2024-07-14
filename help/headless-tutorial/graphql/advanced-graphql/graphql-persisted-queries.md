---
title: 持久GraphQL查询 — AEM Headless的高级概念 — GraphQL
description: 在Adobe Experience Manager (AEM) Headless的高级概念的这一章中，了解如何使用参数创建和更新持久GraphQL查询。 了解如何在持久查询中传递缓存控制参数。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: 6a8e90ae-0765-4066-9df4-a3e4d2cda285
duration: 183
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 1%

---

# 持久 GraphQL 查询

持久查询是存储在Adobe Experience Manager (AEM)服务器上的查询。 客户端可以发送一个具有查询名称的HTTPGET请求来执行它。 这种方法的好处是可缓存性。 虽然客户端GraphQL查询也可以使用HTTPPOST请求执行（无法缓存），但持久查询可以由HTTP缓存或CDN缓存，从而提高性能。 持久查询允许您简化请求并提高安全性，因为您的查询已封装在服务器上，并且AEM管理员可以完全控制它们。 使用AEM GraphQL API时，是&#x200B;**最佳实践并强烈建议**&#x200B;使用持久查询。

在上一章中，您已探索了一些高级GraphQL查询以收集WKND应用程序的数据。 在本章中，您将查询保留到AEM，并了解如何在保留的查询上使用缓存控制。

## 先决条件 {#prerequisites}

本文档是多部分教程的一部分。 在继续本章之前，请确保已完成[上一章](explore-graphql-api.md)。

## 目标 {#objectives}

在本章中，了解如何：

* 使用参数保留GraphQL查询
* 将cache-control参数用于持久查询

## 查看&#x200B;_GraphQL持久查询_&#x200B;配置设置

我们来看看是否已在AEM实例中为WKND站点项目启用&#x200B;_GraphQL持久查询_。

1. 导航到&#x200B;**工具** > **常规** > **配置浏览器**。

1. 选择&#x200B;**WKND共享**，然后在顶部导航栏中选择&#x200B;**属性**&#x200B;以打开配置属性。 在“配置属性”页面上，您应该看到&#x200B;**GraphQL持久查询**&#x200B;权限已启用。

   ![配置属性](assets/graphql-persisted-queries/configuration-properties.png)

## 使用内置GraphiQL Explorer工具持久GraphQL查询

在此部分中，我们将保留GraphQL查询，该查询以后在客户端应用程序中使用来获取和渲染冒险内容片段数据。

1. 在GraphiQL Explorer中输入以下查询：

   ```graphql
   query getAdventureDetailsBySlug($slug: String!) {
   adventureList(filter: {slug: {_expressions: [{value: $slug}]}}) {
       items {
       _path
       title
       activity
       adventureType
       price
       tripLength
       groupSize
       difficulty
       primaryImage {
           ... on ImageRef {
           _path
           mimeType
           width
           height
           }
       }
       description {
           html
           json
       }
       itinerary {
           html
           json
       }
       location {
           _path
           name
           description {
           html
           json
           }
           contactInfo {
           phone
           email
           }
           locationImage {
           ... on ImageRef {
               _path
           }
           }
           weatherBySeason
           address {
           streetAddress
           city
           state
           zipCode
           country
           }
       }
       instructorTeam {
           _metadata {
           stringMetadata {
               name
               value
           }
           }
           teamFoundingDate
           description {
           json
           }
           teamMembers {
           fullName
           contactInfo {
               phone
               email
           }
           profilePicture {
               ... on ImageRef {
               _path
               }
           }
           instructorExperienceLevel
           skills
           biography {
               html
           }
           }
       }
       administrator {
           fullName
           contactInfo {
           phone
           email
           }
           biography {
           html
           }
       }
       }
       _references {
       ... on ImageRef {
           _path
           mimeType
       }
       ... on LocationModel {
           _path
           __typename
       }
       }
   }
   }
   ```

   在保存查询之前，请验证查询是否有效。

1. 接下来，点按另存为，并输入`adventure-details-by-slug`作为查询名称。

   ![保留GraphQL查询](assets/graphql-persisted-queries/persist-graphql-query.png)

## 通过编码特殊字符执行带变量的持久查询

让我们了解客户端应用程序如何通过对特殊字符进行编码来执行带变量的持久查询。

要执行持久查询，客户端应用程序使用以下语法发出GET请求：

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>
```

若要使用变量&#x200B;_执行持久查询_，上述语法将更改为：

```
GET <AEM_HOST>/graphql/execute.json/<Project-Config-Name>/<Persisted-Query-Name>;variable1=value1;variable2=value2
```

必须转换分号(；)、等号(=)、斜杠(/)和空格等特殊字符才能使用相应的UTF-8编码。

通过从命令行终端运行`getAllAdventureDetailsBySlug`查询，我们回顾这些概念的实际操作情况。

1. 打开GraphiQL Explorer并单击永久查询`getAllAdventureDetailsBySlug`旁边的&#x200B;**省略号** (...)，然后单击&#x200B;**复制URL**。 将复制的URL粘贴到文本板中，如下所示：

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=
   ```

1. 将`yosemite-backpacking`添加为变量值

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug;slug=yosemite-backpacking
   ```

1. 编码分号(；)和等号(=)特殊字符

   ```code
       http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

1. 打开命令行终端并使用[Curl](https://curl.se/)运行查询

   ```shell
   $ curl -X GET http://<AEM_HOST>/graphql/execute.json/wknd-shared/getAllAdventureDetailsBySlug%3Bslug%3Dyosemite-backpacking
   ```

>[!TIP]
>
>    如果对AEM创作环境运行上述查询，则必须发送凭据。 请参阅[本地开发访问令牌](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/local-development-access-token.html)进行演示，并参阅[调用AEM API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#calling-the-aem-api)以了解文档详细信息。

此外，请查看[如何执行持久查询](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#execute-persisted-query)、[使用查询变量](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#query-variables)和[对查询URL进行编码以供应用程序](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#encoding-query-url)使用，以了解客户端应用程序如何执行持久查询。

## 更新持久查询中的缓存控制参数 {#cache-control-all-adventures}

AEM GraphQL API允许您更新查询的默认缓存控制参数，以提高性能。 默认的cache-control值为：

* 60秒是客户端（例如浏览器）的默认(maxage=60) TTL

* 7200秒是Dispatcher和CDN的默认(s-maxage=7200) TTL；也称为共享缓存

使用`adventures-all`查询更新缓存控制参数。 查询响应很大，在缓存中控制其`age`很有用。 此持久查询稍后用于更新[客户端应用程序](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md)。

1. 打开GraphiQL Explorer并单击永久查询旁边的&#x200B;**省略号** (...)，然后单击&#x200B;**标头**&#x200B;以打开&#x200B;**缓存配置**&#x200B;模式。

   ![保留GraphQL标头选项](assets/graphql-persisted-queries/persist-graphql-header-option.png)


1. 在&#x200B;**缓存配置**&#x200B;模式中，将`max-age`标头值更新为`600 `秒（10分钟），然后单击&#x200B;**保存**

   ![保留GraphQL缓存配置](assets/graphql-persisted-queries/persist-graphql-cache-config.png)


有关默认缓存控制参数的详细信息，请查看[缓存您的持久查询](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html#caching-persisted-queries)。


## 恭喜！

恭喜！您现在已了解如何使用参数持久GraphQL查询，更新持久查询，以及对持久查询使用缓存控制参数。

## 后续步骤

在[下一章](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md)中，您将在WKND应用程序中实施对持久查询的请求。
