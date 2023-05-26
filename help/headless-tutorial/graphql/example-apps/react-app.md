---
title: React应用程序 — AEM Headless示例
description: 示例应用程序是探索Adobe Experience Manager (AEM)的Headless功能的绝佳方法。 此React应用程序演示了如何使用AEM GraphQL API通过持久化查询来查询内容。
version: Cloud Service
mini-toc-levels: 1
kt: 10715
thumbnail: KT-10715.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-11-09T00:00:00Z
exl-id: b1ab2a13-8b0e-4d7f-82b5-78b1dda248ba
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '916'
ht-degree: 5%

---

# React应用程序{#react-app}

示例应用程序是探索Adobe Experience Manager (AEM)的Headless功能的绝佳方法。 此React应用程序演示了如何使用AEM GraphQL API通过持久化查询来查询内容。 适用于JavaScript的AEM Headless客户端用于执行为应用程序提供支持的GraphQL持久查询。

![使用AEM Headless的React应用程序](./assets/react-app/react-app.png)

查看 [GitHub上的源代码](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)

A [完整的分步教程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=zh-Hans) 提供了描述此React应用程序是如何构建的。

## 前提条件 {#prerequisites}

应在本地安装以下工具：

+ [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=limit&amp;p.limit=0&amp;p.limit=144)
+ [Node.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## AEM要求

React应用程序可与以下AEM部署选项配合使用。 所有部署都需要 [WKND站点v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/tag/aem-guides-wknd-2.1.0) 即将安装。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 使用进行本地设置 [AEM CLOUD SERVICE SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hans)
+ [AEM 6.5 SP13+快速入门](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=zh-Hans？lang=en#install-local-aem-instances)

React应用程序旨在连接到 __AEM发布__ 但是，如果在React应用程序的配置中提供身份验证，则它可以从AEM Author获取内容。

## 使用方法

1. 克隆 `adobe/aem-guides-wknd-graphql` 存储库：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 编辑 `aem-guides-wknd-graphql/react-app/.env.development` 文件和设置 `REACT_APP_HOST_URI` 指向目标AEM。

   如果连接到作者实例，请更新身份验证方法。

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

1. 打开终端并运行以下命令：

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. 新浏览器窗口应加载到 [http://localhost:3000](http://localhost:3000)
1. 应用程序上应显示WKND引用站点中的冒险列表。

## 代码

以下摘要介绍了React应用程序的构建方式、它如何连接到AEM Headless以使用GraphQL持久查询检索内容，以及这些数据的呈现方式。 完整代码可在上找到 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).


### 持久查询

遵循AEM Headless最佳实践，React应用程序使用AEM GraphQL持久查询来查询冒险数据。 应用程序使用两个持久查询：

+ `wknd/adventures-all` 持久查询，该查询返回AEM中的所有冒险以及一组删节的资产。 此持久查询驱动初始视图的冒险列表。

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

+ `wknd/adventure-by-slug` 持久查询，返回一次冒险的方法是 `slug` （唯一标识冒险的自定义属性）和一组完整的属性。 此持久查询支持冒险详细信息视图。

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

AEM持久查询通过HTTPGET执行，因此， [适用于JavaScript的AEM Headless客户端](https://github.com/adobe/aem-headless-client-js) 已用于 [执行持久的GraphQL查询](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany) 针对AEM并将冒险内容加载到应用程序中。

每个持久查询都有一个对应的React [useEffect](https://reactjs.org/docs/hooks-effect.html) 钩入 `src/api/usePersistedQueries.js`，异步调用AEM HTTPGET持久查询终结点并返回冒险数据。

每个函数依次调用 `aemHeadlessClient.runPersistedQuery(...)`，执行持久的GraphQL查询。

```js
// src/api/usePersistedQueries.js

/**
 * React custom hook that returns a list of adevntures by activity. If no activity is provided, all adventures are returned.
 * 
 * Custom hook that calls the 'wknd-shared/adventures-all' or 'wknd-shared/adventures-by-activity' persisted query.
 *
 * @returns an array of Adventure JSON objects, and array of errors
 */
export function useAdventuresByActivity(adventureActivity) {
  ...
  // If an activity is provided (i.e "Camping", "Hiking"...) call wknd-shared/adventures-by-activity query
  if (adventureActivity) {
    // The key is 'activity' as defined in the persisted query
    const queryParameters = { activity: adventureActivity };

    // Call the AEM GraphQL persisted query named "wknd-shared/adventures-by-activity" with parameters
    response = await fetchPersistedQuery("wknd-shared/adventures-by-activity", queryParameters);
  } else {
    // Else call the AEM GraphQL persisted query named "wknd-shared/adventures-all" to get all adventures
    response = await fetchPersistedQuery("wknd-shared/adventures-all");
  }
  
  ... 
}

...
/**
 * Private function that invokes the AEM Headless client.
 * 
 * @param {String} persistedQueryName the fully qualified name of the persisted query
 * @param {*} queryParameters an optional JavaScript object containing query parameters
 * @returns the GraphQL data or an error message 
 */
async function fetchPersistedQuery(persistedQueryName, queryParameters) {
  let data;
  let err;

  try {
    // AEM GraphQL queries are asynchronous, either await their return or use Promise-based .then(..) { ... } syntax
    const response = await aemHeadlessClient.runPersistedQuery(
      persistedQueryName,
      queryParameters
    );
    // The GraphQL data is stored on the response's data field
    data = response?.data;
  } catch (e) {
    // An error occurred, return the error messages
    err = e
      .toJSON()
      ?.map((error) => error.message)
      ?.join(", ");
    console.error(e.toJSON());
  }

  return { data, err };
}
```

### 视图

React应用程序使用两个视图在Web体验中呈现冒险数据。

+ `src/components/Adventures.js`

   调用 `getAdventuresByActivity(..)` 起始日期 `src/api/usePersistedQueries.js` 并将返回的冒险显示在列表中。

+ `src/components/AdventureDetail.js`

   调用 `getAdventureBySlug(..)` 使用 `slug` param通过 `Adventures` 组件，并显示单次冒险的详细信息。

### 环境变量

多个 [环境变量](https://create-react-app.dev/docs/adding-custom-environment-variables) 用于连接到AEM环境。 默认连接到在运行的AEM发布 `http://localhost:4503`. 更新 `.env.development` 文件，更改AEM连接：

+ `REACT_APP_HOST_URI=http://localhost:4502`：设置为AEM目标主机
+ `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json`：设置GraphQL端点路径。 此React应用程序不使用此属性，因为此应用程序仅使用持久查询。
+ `REACT_APP_AUTH_METHOD=`：首选身份验证方法。 可选，根据默认情况，不使用身份验证。
   + `service-token`：使用服务凭据获取AEMas a Cloud Service上的访问令牌
   + `dev-token`：使用开发令牌在AEMas a Cloud Service上进行本地开发
   + `basic`：使用用户/通行证进行本地开发和本地AEM创作
   + 留空以在不进行身份验证的情况下连接到AEM
+ `REACT_APP_AUTHORIZATION=admin:admin`：设置连接到AEM创作环境时使用的基本身份验证凭据（仅用于开发）。 如果连接到“发布”环境，则不需要此设置。
+ `REACT_APP_DEV_TOKEN`：开发令牌字符串。 要连接到远程实例，在基本身份验证(user：pass)旁边，您可以从云控制台将持有者身份验证与开发令牌结合使用
+ `REACT_APP_SERVICE_TOKEN`：服务凭据文件的路径。 要连接到远程实例，还可以使用服务令牌进行身份验证（从开发人员控制台下载文件）。

### 代理AEM请求

使用webpack开发服务器时(`npm start`)项目依赖于 [代理设置](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) 使用 `http-proxy-middleware`. 文件配置于 [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) 并依赖于在设置的多个自定义环境变量 `.env` 和 `.env.development`.

如果连接到AEM创作环境，则相应的 [需要配置身份验证方法](#environment-variables).

### 跨源资源共享(CORS)

此React应用程序依赖于在目标AEM环境中运行的基于AEM的CORS配置，并假定React应用程序在 `http://localhost:3000` 处于开发模式时。 此 [CORS配置](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json) 是 [WKND站点](https://github.com/adobe/aem-guides-wknd).

![CORS配置](assets/react-app/cross-origin-resource-sharing-configuration.png)
