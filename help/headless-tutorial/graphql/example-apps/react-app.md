---
title: React应用程序 — AEM Headless示例
description: 示例应用程序是探索Adobe Experience Manager (AEM)的Headless功能的好方法。 此React应用程序演示了如何使用AEM的GraphQL API通过持久化查询来查询内容。
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
jira: KT-10715
thumbnail: KT-10715.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
exl-id: b1ab2a13-8b0e-4d7f-82b5-78b1dda248ba
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
duration: 256
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '799'
ht-degree: 0%

---

# React应用程序{#react-app}

示例应用程序是探索Adobe Experience Manager (AEM)的Headless功能的好方法。 此React应用程序演示了如何使用AEM的GraphQL API通过持久化查询来查询内容。 适用于JavaScript的AEM Headless客户端用于执行为应用程序提供支持的GraphQL持久查询。

使用AEM Headless的![React应用程序](./assets/react-app/react-app.png)

在GitHub[&#128279;](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)上查看源代码

提供了[完整的分步教程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-Headless/graphql/multi-step/overview.html?lang=zh-Hans)，其中介绍了如何生成此React应用程序。

## 先决条件 {#prerequisites}

应在本地安装以下工具：

+ [Node.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## AEM要求

React应用程序可与以下AEM部署选项配合使用。 所有部署都需要安装[WKND站点v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest)。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 使用[AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-hans)进行本地设置
   + 需要[JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=list&amp;p.offset=0&amp;p.limit=14&amp;p.limit=144)

React应用程序旨在连接到&#x200B;__AEM Publish__&#x200B;环境，但是，如果在React应用程序的配置中提供身份验证，则该应用程序可以从AEM Author获取内容。

## 使用方法

1. 克隆`adobe/aem-guides-wknd-graphql`存储库：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 编辑`aem-guides-wknd-graphql/react-app/.env.development`文件并将`REACT_APP_HOST_URI`设置为指向您的目标AEM。

   如果连接到作者实例，请更新身份验证方法。

   ```plain
   # Server namespace
   REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com
   
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

1. 新的浏览器窗口应在[http://localhost:3000](http://localhost:3000)上加载
1. 应用程序上应显示WKND引用站点中的冒险列表。

## 代码

以下摘要介绍了React应用程序的构建方式、它如何连接到AEM Headless以使用GraphQL持久查询检索内容，以及这些数据的呈现方式。 可在[GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)上找到完整代码。


### 持久查询

遵循AEM Headless最佳实践，React应用程序使用AEM GraphQL持久查询来查询冒险数据。 该应用程序使用两个持久查询：

+ `wknd/adventures-all`持久查询，该查询返回AEM中的所有冒险，并包含属性删节集。 此持久查询驱动初始视图的冒险列表。

```
# Retrieves a list of all Adventures
#
# Optional query variables:
# - { "offset": 10 }
# - { "limit": 5 }
# - { 
#    "imageFormat": "JPG",
#    "imageWidth": 1600,
#    "imageQuality": 90 
#   }
query ($offset: Int, $limit: Int, $sort: String, $imageFormat: AssetTransformFormat=JPG, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    offset: $offset
    limit: $limit
    sort: $sort
    _assetTransform: {
      format: $imageFormat
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
  }) {
    items {
      _path
      slug
      title
      activity
      price
      tripLength
      primaryImage {
        ... on ImageRef {
          _path
          _dynamicUrl
        }
      }
    }
  }
}
```

+ `wknd/adventure-by-slug`持久查询，该查询返回由`slug`（唯一标识冒险的自定义属性）通过一组完整属性进行的单次冒险。 此持久查询为冒险详细信息视图提供支持。

```
# Retrieves an Adventure Fragment based on it's unique slug.
#
# Required query variables:
# - {"slug": "bali-surf-camp"}
#
# Optional query variables:
# - { 
#     "imageFormat": "JPG",
#     "imageSeoName": "my-adventure",
#     "imageWidth": 1600,
#     "imageQuality": 90 
#   }
#  
# This query returns an adventure list but since the the slug property is set to be unique in the Content Fragment Model, only a single Content Fragment is expected.

query ($slug: String!, $imageFormat:AssetTransformFormat=JPG, $imageSeoName: String, $imageWidth: Int=1200, $imageQuality: Int=80) {
  adventureList(
    filter: {slug: {_expressions: [{value: $slug}]}}
    _assetTransform: {
      format: $imageFormat
      seoName: $imageSeoName
      width: $imageWidth
      quality: $imageQuality
      preferWebp: true
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
          _dynamicUrl
        }
      }
      description {
        json
        plaintext
        html
      }
      itinerary {
        json
        plaintext
        html
      }
    }
    _references {
      ... on AdventureModel {
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

AEM的持久查询通过HTTP GET执行，因此，适用于JavaScript[&#128279;](https://github.com/adobe/aem-headless-client-js)的AEM Headless客户端用于[对AEM执行持久的GraphQL查询](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany)，并将冒险内容加载到应用程序中。

每个持久查询在`src/api/usePersistedQueries.js`中具有相应的React [useEffect](https://reactjs.org/docs/hooks-effect.html)挂接，该挂接异步调用AEM HTTP GET持久查询终结点，并返回冒险数据。

每个函数依次调用`aemHeadlessClient.runPersistedQuery(...)`，执行持久的GraphQL查询。

```js
// src/api/usePersistedQueries.js

/**
 * React custom hook that returns a list of adevntures by activity. If no activity is provided, all adventures are returned.
 * 
 * Custom hook that calls the 'wknd-shared/adventures-all' or 'wknd-shared/adventures-by-activity' persisted query.
 *
 * @returns an array of Adventure JSON objects, and array of errors
 */
export function useAdventuresByActivity(adventureActivity, params = {}) {
  ...
  let queryVariables = params;

  // If an activity is provided (i.e "Camping", "Hiking"...) call wknd-shared/adventures-by-activity query
  if (adventureActivity) {
    // The key is 'activity' as defined in the persisted query
    queryVariables = { ...queryVariables, activity: adventureActivity };

    // Call the AEM GraphQL persisted query named "wknd-shared/adventures-by-activity" with parameters
    response = await fetchPersistedQuery("wknd-shared/adventures-by-activity", queryVariables);
  } else {
    // Else call the AEM GraphQL persisted query named "wknd-shared/adventures-all" to get all adventures
    response = await fetchPersistedQuery("wknd-shared/adventures-all", queryVariables);
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

React应用程序使用两个视图在Web体验中展示冒险数据。

+ `src/components/Adventures.js`

  从`src/api/usePersistedQueries.js`中调用`getAdventuresByActivity(..)`并在列表中显示返回的冒险。

+ `src/components/AdventureDetail.js`

  使用通过`Adventures`组件上的冒险选择传入的`slug`参数调用`getAdventureBySlug(..)`，并显示单个冒险的详细信息。

### 环境变量

多个[环境变量](https://create-react-app.dev/docs/adding-custom-environment-variables)用于连接到AEM环境。 默认连接到在`http://localhost:4503`运行的AEM Publish。 更新`.env.development`文件以更改AEM连接：

+ `REACT_APP_HOST_URI=https://publish-p123-e456.adobeaemcloud.com`：设置为AEM目标主机
+ `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json`：设置GraphQL终结点路径。 此React应用程序不使用此选项，因为此应用程序仅使用持久化查询。
+ `REACT_APP_AUTH_METHOD=`：首选的身份验证方法。 可选，根据默认情况，不使用身份验证。
   + `service-token`：使用服务凭据获取AEM as a Cloud Service上的访问令牌
   + `dev-token`：在AEM as a Cloud Service上使用开发令牌进行本地开发
   + `basic`：将用户/通行证用于本地AEM Author的本地开发
   + 留空以在不进行身份验证的情况下连接到AEM
+ `REACT_APP_AUTHORIZATION=admin:admin`：设置连接到AEM创作环境时要使用的基本身份验证凭据（仅用于开发）。 如果连接到“发布”环境，则不需要此设置。
+ `REACT_APP_DEV_TOKEN`：开发令牌字符串。 要连接到远程实例，在基本身份验证(user：pass)旁边，您可以从云控制台将持有者身份验证与开发令牌结合使用
+ `REACT_APP_SERVICE_TOKEN`：服务凭据文件的路径。 要连接到远程实例，还可以使用服务令牌进行身份验证(从Developer Console下载文件)。

### 代理AEM请求

使用webpack开发服务器(`npm start`)时，项目依赖于使用`http-proxy-middleware`的[代理设置](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually)。 该文件在[src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js)中配置，并且依赖在`.env`和`.env.development`设置的多个自定义环境变量。

如果连接到AEM创作环境，则需要配置相应的[身份验证方法](#environment-variables)。

### 跨源资源共享(CORS)

此React应用程序依赖于在目标AEM环境中运行的基于AEM的CORS配置，并假定React应用程序在开发模式下在`http://localhost:3000`上运行。  查看[AEM Headless部署文档](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa.html)，了解有关如何设置和配置CORS的更多信息。
