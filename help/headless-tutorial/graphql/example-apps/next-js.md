---
title: Next.js - AEM Headless示例
description: 示例应用程序是探索Adobe Experience Manager(AEM)无头功能的绝佳方式。 此Next.js应用程序演示了如何使用AEM GraphQL API使用持久查询来查询内容。
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10721
thumbnail: KT-10721.jpg
last-substantial-update: 2022-10-03T00:00:00Z
source-git-commit: 1ecd3c761ea7c79036b263ff8528a6cd01af0e76
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 1%

---

# Next.js应用程序

示例应用程序是探索Adobe Experience Manager(AEM)无头功能的绝佳方式。 此Next.js应用程序演示了如何使用AEM GraphQL API使用持久查询来查询内容。 用于JavaScript的AEM无头客户端用于执行为应用程序提供动力的GraphQL持久查询。

![包含AEM Headless的Next.js应用程序](./assets/next-js/next-js.png)

查看 [GitHub上的源代码](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js)

## 前提条件 {#prerequisites}

应在本地安装以下工具：

+ [Node.js v16+](https://nodejs.org/en/)
+ [npm 8+](https://www.npmjs.com/)
+ [Git](https://git-scm.com/)

## AEM要求

Next.js应用程序可与以下AEM部署选项配合使用。 所有部署都需要 [WKND共享v2.1.0+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest), [WKND Site v2.1.0+](https://github.com/adobe/aem-guides-wknd/releases/latest)，或 [参考演示附加组件](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/demo-add-on/overview.html) 安装在AEMas a Cloud Service环境中。

此示例Next.js应用程序旨在连接到 __AEM发布__ 服务。

### AEM创作要求

Next.js旨在连接到 __AEM发布__ 服务，并访问不受保护的内容。 Next.js可配置为通过 `.env` 属性。 从AEM作者提供的图像需要进行身份验证，因此访问Next.js应用程序的用户还必须登录到AEM作者。

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

   如果连接到AEM创作服务，则必须提供身份验证，因为AEM创作服务默认是安全的。

   使用本地AEM帐户集 `AEM_AUTH_METHOD=basic` ，并在 `AEM_AUTH_USER` 和 `AEM_AUTH_PASSWORD` 属性。

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=basic
   AEM_AUTH_USER=aem-user-account 
   AEM_AUTH_PASSWORD=password-for-the-aem-user-account
   ```

   使用 [AEMas a Cloud Service本地开发令牌](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#generating-the-access-token) set `AEM_AUTH_METHOD=dev-token` 和在 `AEM_AUTH_DEV_TOKEN` 属性。

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=dev-token
   AEM_AUTH_DEV_TOKEN=my-dev-token
   ```

1. 编辑 `aem-guides-wknd-graphql/next-js/.env.local` 文件和验证  `NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT` 设置为相应的AEM GraphQL端点。

   使用 [WKND共享](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) 或 [WKND站点](https://github.com/adobe/aem-guides-wknd/releases/latest)，则使用 `wknd-shared` GraphQL API端点。

   ```plain
   ...
   NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT=wknd-shared
   ...
   ```

   使用 [参考演示附加组件](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/demo-add-on/overview.html)，则使用 `aem-demo-assets` GraphQL API端点。

   ```plain
   ...
   NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT=aem-demo-assets
   ...
   ```

1. 打开命令提示符，然后使用以下命令启动Next.js应用程序：

   ```shell
   $ cd aem-guides-wknd-graphql/next-js
   $ npm install
   $ npm run dev
   ```

1. 新的浏览器窗口将在 [http://localhost:3000](http://localhost:3000)
1. Next.js应用程序会显示历险列表。 选择冒险可在新页面中打开其详细信息。

## 代码

以下是如何构建Next.js应用程序、如何连接到AEM Headless以使用GraphQL持久查询检索内容，以及如何显示该数据的摘要。 完整代码可在 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js).

### 持久化查询

遵循AEM无头最佳实践，Next.js应用程序使用AEM GraphQL持久查询来查询冒险数据。 应用程序使用两个持久查询：

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

每个持久化查询在 `src/lib//aem-headless-client.js`，可调用AEM GraphQL端点并返回冒险数据。

每个函数依次调用 `aemHeadlessClient.runPersistedQuery(...)`，执行持久GraphQL查询。

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

async getAdventureSlugs() { ... }

async getAdventuresBySlug(slug) { ... }
...
```

### 页面

Next.js应用程序使用两个页面来显示冒险数据。

+ `src/pages/index.js`

   使用 [Next.js的getServerSideProps()](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props) 调用 `getAllAdventures()` 将每个冒险都显示为卡片。

   的使用 `getServerSiteProps()` 允许在服务器端渲染此Next.js页面。

+ `src/pages/adventures/[...slug].js`

   A [Next.js动态路由](https://nextjs.org/docs/routing/dynamic-routes) 显示单次冒险的细节。 此动态路由使用 [Next.js的getStaticProps()](https://nextjs.org/docs/basic-features/data-fetching/get-static-props) 通过 `getAdventureBySlug(..)` 使用 `slug` 通过 `adventures/index.js` 页面。

   动态路由能够使用 [Next.js的getStaticPaths()](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths) 并根据GraphQL查询返回的完整历程列表填充所有可能的路由排列  `getAdventurePaths()`

   的使用 `getStaticPaths()` 和 `getStaticProps(..)` 允许静态站点生成这些Next.js页面。

## 部署配置

Next.js应用程序，特别是在服务器端渲染(SSR)和服务器端生成(SSG)的上下文中，不需要高级安全配置，例如跨域资源共享(CORS)。

但是，如果Next.js确实从客户端的上下文向AEM发出HTTP请求，则可能需要AEM中的安全配置。 查看 [AEM Headless单页应用程序部署教程](../deployment/spa.md) 以了解更多详细信息。

