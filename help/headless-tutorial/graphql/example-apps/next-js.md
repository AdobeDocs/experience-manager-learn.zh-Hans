---
title: Next.js - AEM Headless示例
description: 示例应用程序是探索Adobe Experience Manager (AEM)的Headless功能的绝佳方法。 此Next.js应用程序演示了如何使用AEM GraphQL API通过持久查询来查询内容。
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10721
thumbnail: KT-10721.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headlessas a Cloud Service" before-title="false"
exl-id: 4f67bb37-416a-49d9-9d7b-06c3573909ca
duration: 234
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '744'
ht-degree: 0%

---

# Next.js应用程序

示例应用程序是探索Adobe Experience Manager (AEM)的Headless功能的绝佳方法。 此Next.js应用程序演示了如何使用AEM GraphQL API通过持久查询来查询内容。 适用于JavaScript的AEM Headless Client用于执行为应用程序提供支持的GraphQL持久查询。

![使用AEM Headless的Next.js应用程序](./assets/next-js/next-js.png)

查看 [GitHub上的源代码](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js)

## 前提条件 {#prerequisites}

应在本地安装以下工具：

+ [Node.js v18](https://nodejs.org/)
+ [Git](https://git-scm.com/)

## AEM要求

Next.js应用程序可与以下AEM部署选项配合使用。 所有部署都需要 [WKND共享版本3.0.0+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) 或 [WKND站点v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) ，以安装在AEMas a Cloud Service环境中。

此示例Next.js应用程序旨在连接到 __AEM发布__ 服务。

### AEM创作要求

Next.js旨在连接到 __AEM发布__ 服务，并访问不受保护的内容。 AEM Next.js可以配置为通过 `.env` 属性如下所述。 由AEM Author提供的图像需要进行身份验证，因此访问Next.js应用程序的用户还必须登录到AEM Author。

## 使用方法

1. 克隆 `adobe/aem-guides-wknd-graphql` 存储库：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 编辑 `aem-guides-wknd-graphql/next-js/.env.local` 文件和设置 `NEXT_PUBLIC_AEM_HOST` 到AEM服务。

   ```plain
   # AEM service
   NEXT_PUBLIC_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com/
   ...
   ```

   如果连接到AEM Author服务，则必须提供身份验证，因为默认情况下AEM Author服务是安全的。

   使用本地AEM帐户集 `AEM_AUTH_METHOD=basic` 并在以下位置提供用户名和密码： `AEM_AUTH_USER` 和 `AEM_AUTH_PASSWORD` 属性。

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=basic
   AEM_AUTH_USER=aem-user-account 
   AEM_AUTH_PASSWORD=password-for-the-aem-user-account
   ```

   要使用 [AEMas a Cloud Service本地开发令牌](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#generating-the-access-token) 设置 `AEM_AUTH_METHOD=dev-token` 并在中提供完整的开发令牌值 `AEM_AUTH_DEV_TOKEN` 属性。

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=dev-token
   AEM_AUTH_DEV_TOKEN=my-dev-token
   ```

1. 编辑 `aem-guides-wknd-graphql/next-js/.env.local` 文件并验证  `NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT` 设置为相应的AEM GraphQL端点。

   使用时 [WKND已共享](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) 或 [WKND站点](https://github.com/adobe/aem-guides-wknd/releases/latest)，使用 `wknd-shared` GraphQL API端点。

   ```plain
   ...
   NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT=wknd-shared
   ...
   ```

1. 打开命令提示符并使用以下命令启动Next.js应用程序：

   ```shell
   $ cd aem-guides-wknd-graphql/next-js
   $ npm install
   $ npm run dev
   ```

1. 新的浏览器窗口将在以下位置打开Next.js应用程序： [http://localhost:3000](http://localhost:3000)
1. Next.js应用程序显示冒险列表。 选择冒险将在新页面中打开其详细信息。

## 代码

以下摘要介绍了如何构建Next.js应用程序，它如何连接到AEM Headless以使用GraphQL持久查询检索内容，以及数据如何呈现。 完整代码可在上找到 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js).

### 持久查询

遵循AEM Headless最佳实践，Next.js应用程序使用AEM GraphQL持久查询来查询冒险数据。 应用程序使用两个持久查询：

+ `wknd/adventures-all` 持久查询，该查询返回在AEM中使用一组删节的属性进行的所有冒险。 此持久查询驱动初始视图的冒险列表。

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

+ `wknd/adventure-by-slug` 持久查询，返回一次冒险 `slug` （唯一标识冒险的自定义属性）和一组完整的属性。 此持久查询为冒险详细信息视图提供支持。

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

AEM持久查询通过HTTPGET执行，因此， [适用于JavaScript的AEM Headless客户端](https://github.com/adobe/aem-headless-client-js) 用于 [执行持久的GraphQL查询](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany) 针对AEM将冒险内容加载到应用程序中。

每个持久查询在中都有相应的函数 `src/lib//aem-headless-client.js`调用AEM GraphQL端点并返回冒险数据。

每个函数依次调用 `aemHeadlessClient.runPersistedQuery(...)`，执行持久的GraphQL查询。

```js
// src/lib/aem-headless-client.js

...
/**
 * Invokes the 'adventures-all` persisted query using the parameterizable namespace.
 * 
 * @returns a GraphQL response of all adventures.
 */
async getAllAdventures() {
  const queryAdventuresAll = process.env.NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT + '/adventures-all';
    
  try {
    return await this.aemHeadlessClient.runPersistedQuery(queryAdventuresAll);
  } catch(e) {
    console.error(e)
  }    
}

// And so on, and so forth ... 

async getAdventureSlugs(queryVariables) { ... }

async getAdventuresBySlug(slug, queryVariables) { ... }
...
```

### 页面

Next.js应用程序使用两个页面展示冒险数据。

+ `src/pages/index.js`

  用途 [Next.js的getServerSideProps()](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props) 以调用 `getAllAdventures()` 并把每一次冒险都当做一张卡片来展示。

  使用 `getServerSiteProps()` 允许在服务器端呈现此Next.js页面。

+ `src/pages/adventures/[...slug].js`

  A [Next.js动态路由](https://nextjs.org/docs/routing/dynamic-routes) 显示单次冒险的细节。 此动态路由会使用预取每个冒险的数据 [Next.js的getStaticProps()](https://nextjs.org/docs/basic-features/data-fetching/get-static-props) 通过调用 `getAdventureBySlug(slug, queryVariables)` 使用 `slug` param通过 `adventures/index.js` 页面，以及 `queryVariables` 控制图像格式、宽度和质量。

  动态路由能够使用预取所有冒险的详细信息 [Next.js的getStaticPaths()](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths) 并根据GraphQL查询返回的冒险完整列表填充所有可能的路由排列  `getAdventurePaths()`

  使用 `getStaticPaths()` 和 `getStaticProps(..)` 允许生成这些Next.js页面的静态站点。

## 部署配置

Next.js应用程序，尤其是在服务器端渲染(SSR)和服务器端生成(SSG)的上下文中，不需要高级安全配置，例如跨源资源共享(CORS)。

但是，如果Next.js确实从客户端上下文中向AEM发出HTTP请求，则可能需要AEM中的安全配置。 查看 [AEM Headless单页应用程序部署教程](../deployment/spa.md) 以了解更多详细信息。
