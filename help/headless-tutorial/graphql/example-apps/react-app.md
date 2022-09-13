---
title: React应用程序 — AEM Headless示例
description: 示例应用程序是探索Adobe Experience Manager(AEM)无头功能的绝佳方式。 此React应用程序演示了如何使用AEM GraphQL API通过持久查询来查询内容。
version: Cloud Service
mini-toc-levels: 1
kt: 10715
thumbnail: KT-10715.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: b1ab2a13-8b0e-4d7f-82b5-78b1dda248ba
source-git-commit: b2bf2a8e454d7ccd09819f2a38e58f7c303cb066
workflow-type: tm+mt
source-wordcount: '912'
ht-degree: 4%

---

# React App

示例应用程序是探索Adobe Experience Manager(AEM)无头功能的绝佳方式。 此React应用程序演示了如何使用AEM GraphQL API通过持久查询来查询内容。 用于JavaScript的AEM无头客户端用于执行为应用程序提供动力的GraphQL持久查询。

![使用AEM Headless反应应用程序](./assets/react-app/react-app.png)

查看 [GitHub上的源代码](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)

A [分步完整教程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=zh-Hans) 描述此React应用程序的生成方式。

## 前提条件 {#prerequisites}

应在本地安装以下工具：

+ [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Cont%2Fjcr%3Alast&amp;by.sort=desc&amp;list=0&amp;p.offset=14)
+ [Node.js v10+](https://nodejs.org/en/)
+ [npm 6+](https://www.npmjs.com/)
+ [Git](https://git-scm.com/)

## AEM要求

React应用程序可与以下AEM部署选项配合使用。 所有部署都需要 [WKND Site v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) 安装。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 使用进行本地设置 [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)
+ [AEM 6.5 SP13+快速入门](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances)

React应用程序旨在连接到 __AEM发布__ 环境中，但是，如果在React应用程序配置中提供了身份验证，则可以从AEM Author中源内容。

## 使用方法

1. 克隆 `adobe/aem-guides-wknd-graphql` 存储库：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 编辑 `aem-guides-wknd-graphql/react-app/.env.development` 文件和设置 `REACT_APP_HOST_URI` 来指向您的target AEM。

   如果连接到创作实例，请更新身份验证方法。

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=http://localhost:4503
   
   #AUTH (Choose one method)
   # Authentication methods: 'service-token', 'dev-token', 'basic' or leave blank to use no authentication
   REACT_APP_AUTH_METHOD=basic
   
   # For Bearer auth, use DEV token (dev-token) from Cloud console
   REACT_APP_DEV_TOKEN=dev-token
   
   # For Service toke auth, provide path to service token file (download file from Cloud console)
   REACT_APP_SERVICE_TOKEN=auth/service-token.json
   
   # For Basic auth, use AEM ['user','pass'] pair (eg for Local AEM Author instance)
   REACT_APP_BASIC_AUTH_USER=admin
   REACT_APP_BASIC_AUTH_PASS=admin
   ```

1. 打开终端并运行命令：

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. 应加载新的浏览器窗口 [http://localhost:3000](http://localhost:3000)
1. 应用程序上应显示来自WKND引用站点的历险列表。

## 代码

以下是如何构建React应用程序的摘要，它如何连接到AEM Headless以使用GraphQL持久查询检索内容，以及如何显示该数据。 完整代码可在 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).


### 持久化查询

遵循AEM无头最佳实践，React应用程序使用AEM GraphQL持久查询来查询冒险数据。 应用程序使用两个持久查询：

+ `wknd/adventures-all` 持久查询，该查询会返回具有一组简略属性的AEM中的所有冒险。 此持久查询驱动初始视图的探险列表。

```
# Retrieves a list of all adventures
{
    adventureList {
        items {
            _path
            slug
            title
            price
            tripLength
            primaryImage {
                ... on ImageRef {
                _path
                mimeType
                width
                height
                }
            }
        }
    }
}
```

+ `wknd/adventure-by-slug` 持久查询，返回单个冒险项 `slug` （一个自定义属性，用于唯一标识冒险），具有一组完整的属性。 此持久查询支持探险详细信息视图。

```
# Retrieves an adventure Content Fragment based on it's slug
# Example query variables: 
# {"slug": "bali-surf-camp"} 
# Technically returns an adventure list but since the the slug 
# property is set to be unique in the CF Model, only a single CF is expected

query($slug: String!) {
  adventureList(filter: {
        slug: {
          _expressions: [ { value: $slug } ]
        }
  	}) {
    items {
      _path
      title
      slug
      activity
      adventureType
      price
      tripLength
      groupSize
      difficulty
      price
      primaryImage {
        ... on ImageRef {
          _path
          mimeType
          width
          height
        }
      }
      description {
        json
        plaintext
      }
      itinerary {
        json
        plaintext
      }
    }
    _references {
      ...on AdventureModel {
        _path
        slug
        title
        price
        __typename
      }
    }
  }
}
```

### 执行GraphQL持久查询

AEM持久查询是通过HTTPGET执行的，因此， [AEM JavaScript无头客户端](https://github.com/adobe/aem-headless-client-js) 用于 [执行持久GraphQL查询](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany) 并将冒险内容加载到应用程序中。

每个持久化查询在 `src/api/persistedQueries.js`，可异步调用AEM HTTPGET端点并返回冒险数据。

每个函数依次调用 `aemHeadlessClient.runPersistedQuery(...)`，执行持久GraphQL查询。

```js
// src/api/persistedQueries.js

/**
 * Queries a list of all Adventures using the persisted path "wknd-shared/adventures-all"
 * @returns {data, errors}
 */
export const getAllAdventures = async function() {
    return executePersistedQuery('wknd-shared/adventures-all');
}

...

/**
 * Uses the AEM Headless SDK to execute a query besed on a persistedQueryPath and optional query variables
 * @param {*} persistedQueryPath 
 * @param {*} queryVariables 
 * @returns 
 */
 const executePersistedQuery = async function(persistedQueryPath, queryVariables) {

    let data;
    let errors;

    try {
        // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryPath, queryVariables);
        // The GraphQL data is stored on the response's data field
        data = response.data;
        errors = response.errors ? mapErrors(response.errors) : undefined;
    } catch (e) {
        console.error(e.toJSON());
        errors = e;
    }

    return {data, errors}; 
}
```

### 视图

React应用程序使用两个视图来呈现Web体验中的冒险数据。

+ `src/components/Adventures.js`

   调用 `getAllAdventures()` 从 `src/api/persistedQueries.js`  并在列表中显示返回的冒险。

+ `src/components/AdventureDetail.js`

   调用 `getAdventureBySlug(..)` 使用 `slug` 通过 `Adventures` 组件，并显示单个冒险的详细信息。


### 环境变量

几个 [环境变量](https://create-react-app.dev/docs/adding-custom-environment-variables) 用于连接到AEM环境。 默认情况下，会连接到运行在的AEM发布 `http://localhost:4503`. 要更改AEM连接，请更新 `.env.development` 文件：

+ `REACT_APP_HOST_URI=http://localhost:4502`:设置为AEM Target主机
+ `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json`:设置GraphQL端点路径。 此React应用程序不使用此函数，因为此应用程序仅使用持久查询。
+ `REACT_APP_AUTH_METHOD=`:首选的身份验证方法。 可选，默认情况下不使用身份验证。
   + `service-token`:使用服务凭据在AEMas a Cloud Service上获取访问令牌
   + `dev-token`:在AEMas a Cloud Service上使用开发令牌进行本地开发
   + `basic`:通过本地AEM作者使用用户/通行证进行本地开发
   + 留空以在未进行身份验证的情况下连接到AEM
+ `REACT_APP_AUTHORIZATION=admin:admin`:设置在连接到AEM创作环境时使用的基本身份验证凭据（仅用于开发）。 如果连接到发布环境，则不需要此设置。
+ `REACT_APP_DEV_TOKEN`:开发令牌字符串。 要连接到远程实例，除了基本身份验证(user:pass)之外，您还可以在云控制台中将载体身份验证与DEV令牌结合使用
+ `REACT_APP_SERVICE_TOKEN`:服务凭据文件的路径。 要连接到远程实例，还可以使用服务令牌（从开发人员控制台下载文件）完成身份验证。

### 代理AEM请求

使用WebPack开发服务器(`npm start`)项目依赖于 [代理设置](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) 使用 `http-proxy-middleware`. 文件配置在 [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) 并依赖于 `.env` 和 `.env.development`.

如果连接到AEM创作环境，则对应的 [验证方法需要配置](#environment-variables).

### 跨源资源共享(CORS)

此React应用程序依赖于在目标AEM环境中运行的基于AEM的CORS配置，并假定React应用程序在上运行 `http://localhost:3000` 在开发模式下。 的 [CORS配置](https://github.com/adobe/aem-guides-wknd/blob/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json) 是 [WKND站点](https://github.com/adobe/aem-guides-wknd).

![CORS配置](assets/react-app/cross-origin-resource-sharing-configuration.png)
