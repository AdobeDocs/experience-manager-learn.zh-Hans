---
title: Next.js - AEM Headless範例
description: 範例應用程式是探索Adobe Experience Manager (AEM)的Headless功能的絕佳方式。 此Next.js應用程式示範了如何使用AEM GraphQL API透過持久查詢來查詢內容。
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10721
thumbnail: KT-10721.jpg
last-substantial-update: 2022-10-03T00:00:00Z
exl-id: 4f67bb37-416a-49d9-9d7b-06c3573909ca
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '802'
ht-degree: 1%

---

# Next.js應用程式

範例應用程式是探索Adobe Experience Manager (AEM)的Headless功能的絕佳方式。 此Next.js應用程式示範了如何使用AEM GraphQL API透過持久查詢來查詢內容。 適用於JavaScript的AEM Headless Client用於執行GraphQL持續查詢，為應用程式提供支援。

![使用AEM Headless的Next.js應用程式](./assets/next-js/next-js.png)

檢視 [GitHub上的原始程式碼](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js)

## 前提条件 {#prerequisites}

下列工具應安裝在本機：

+ [Node.js v18](https://nodejs.org/)
+ [Git](https://git-scm.com/)

## AEM需求

Next.js應用程式可與下列AEM部署選項搭配使用。 所有部署都需要 [WKND共用v2.1.0+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) 或 [WKND網站v2.1.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) 安裝在AEMas a Cloud Service環境中。

此範例Next.js應用程式設計用來連線至 __AEM發佈__ 服務。

### AEM作者需求

Next.js的設計目的是為了連線至 __AEM發佈__ 服務，並存取未受保護的內容。 Next.js可設定為透過以下方式連線到AEM Author： `.env` 屬性，如下所述。 由AEM Author提供的影像需要驗證，因此存取Next.js應用程式的使用者也必須登入AEM Author。

## 使用方式

1. 原地複製 `adobe/aem-guides-wknd-graphql` 存放庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 編輯 `aem-guides-wknd-graphql/next-js/.env.local` 檔案和集合 `NEXT_PUBLIC_AEM_HOST` AEM服務。

   ```plain
   # AEM service
   NEXT_PUBLIC_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com/
   ...
   ```

   如果連線到AEM Author服務，則必須提供驗證，因為AEM Author服務預設是安全的。

   若要使用本機AEM帳戶集 `AEM_AUTH_METHOD=basic` 並在中提供使用者名稱和密碼 `AEM_AUTH_USER` 和 `AEM_AUTH_PASSWORD` 屬性。

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=basic
   AEM_AUTH_USER=aem-user-account 
   AEM_AUTH_PASSWORD=password-for-the-aem-user-account
   ```

   若要使用 [AEMas a Cloud Service本機開發權杖](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html#generating-the-access-token) set `AEM_AUTH_METHOD=dev-token` 並在中提供完整的開發權杖值 `AEM_AUTH_DEV_TOKEN` 屬性。

   ```plain
   ...
   # The variables are not prefixed with NEXT_PUBLIC so they are only available server-side
   AEM_AUTH_METHOD=dev-token
   AEM_AUTH_DEV_TOKEN=my-dev-token
   ```

1. 編輯 `aem-guides-wknd-graphql/next-js/.env.local` 檔案與驗證  `NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT` 設為適當的AEM GraphQL端點。

   使用時 [WKND已共用](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) 或 [WKND網站](https://github.com/adobe/aem-guides-wknd/releases/latest)，使用 `wknd-shared` GraphQL API端點。

   ```plain
   ...
   NEXT_PUBLIC_AEM_GRAPHQL_ENDPOINT=wknd-shared
   ...
   ```

1. 開啟命令提示字元，並使用下列命令啟動Next.js應用程式：

   ```shell
   $ cd aem-guides-wknd-graphql/next-js
   $ npm install
   $ npm run dev
   ```

1. 新的瀏覽器視窗會開啟Next.js應用程式，網址為 [http://localhost:3000](http://localhost:3000)
1. Next.js應用程式會顯示冒險清單。 選取冒險會在新頁面中開啟其詳細資訊。

## 程式碼

以下是如何建立Next.js應用程式、它如何連線到AEM Headless以使用GraphQL持久查詢擷取內容，以及資料如何呈現的摘要。 完整程式碼可在上找到 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/next-js).

### 持久查詢

依照AEM Headless最佳實務，Next.js應用程式會使用AEM GraphQL持續查詢來查詢冒險資料。 應用程式使用兩個持續查詢：

+ `wknd/adventures-all` 持久查詢，這會傳回AEM中所有冒險和屬性刪節集。 此持續查詢會驅動初始檢視的冒險清單。

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

+ `wknd/adventure-by-slug` 持久查詢，會依據以下條件傳回單一冒險 `slug` （可唯一識別冒險的自訂屬性）和完整的屬性集。 此持續查詢可支援冒險詳細資料檢視。

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

### 執行GraphQL持續查詢

AEM持續查詢會透過HTTPGET執行，因此， [適用於JavaScript的AEM Headless使用者端](https://github.com/adobe/aem-headless-client-js) 用於 [執行持續的GraphQL查詢](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md#aemheadlessrunpersistedquerypath-variables-options--promiseany) 針對AEM，並將冒險內容載入應用程式。

每個持續查詢在中都有一個對應的函式 `src/lib//aem-headless-client.js`，會呼叫AEM GraphQL端點，並傳回冒險資料。

每個函式又會叫用 `aemHeadlessClient.runPersistedQuery(...)`，執行持續的GraphQL查詢。

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

Next.js應用程式使用兩個頁面來呈現冒險資料。

+ `src/pages/index.js`

   使用 [Next.js的getServerSideProps()](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props) 以呼叫 `getAllAdventures()` 和會將每個冒險都顯示為卡片。

   使用 `getServerSiteProps()` 允許伺服器端轉譯此Next.js頁面。

+ `src/pages/adventures/[...slug].js`

   A [Next.js動態路由](https://nextjs.org/docs/routing/dynamic-routes) 可顯示單一冒險的詳細資訊。 此動態路由會使用預先擷取每個冒險的資料 [Next.js的getStaticProps()](https://nextjs.org/docs/basic-features/data-fetching/get-static-props) 透過呼叫 `getAdventureBySlug(..)` 使用 `slug` 引數是透過 `adventures/index.js` 頁面。

   動態路由能夠使用，預先擷取所有冒險的詳細資料 [Next.js的getStaticPaths()](https://nextjs.org/docs/basic-features/data-fetching/get-static-paths) 並根據GraphQL查詢傳回的冒險完整清單填入所有可能的路徑排列  `getAdventurePaths()`

   使用 `getStaticPaths()` 和 `getStaticProps(..)` 允許靜態網站產生這些Next.js頁面。

## 部署設定

Next.js應用程式，尤其是在伺服器端轉譯(SSR)和伺服器端產生(SSG)的內容中，不需要進階安全性設定，例如跨原始資源共用(CORS)。

不過，如果Next.js確實從使用者端內容向AEM發出HTTP請求，則可能需要AEM中的安全性設定。 檢閱 [AEM Headless單頁應用程式部署教學課程](../deployment/spa.md) 以取得更多詳細資料。
