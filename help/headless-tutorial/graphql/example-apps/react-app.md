---
title: React應用程式 — AEM Headless範例
description: 範例應用程式是探索Adobe Experience Manager (AEM)的Headless功能的絕佳方式。 此React應用程式示範了如何使用AEM GraphQL API透過持續性查詢來查詢內容。
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

# React應用程式{#react-app}

範例應用程式是探索Adobe Experience Manager (AEM)的Headless功能的絕佳方式。 此React應用程式示範了如何使用AEM GraphQL API透過持續性查詢來查詢內容。 適用於JavaScript的AEM Headless Client用於執行GraphQL持續查詢，為應用程式提供支援。

![使用AEM Headless的React應用程式](./assets/react-app/react-app.png)

檢視 [GitHub上的原始程式碼](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app)

A [完整逐步教學課程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=zh-Hans) 說明此React應用程式的建置方式。

## 前提条件 {#prerequisites}

下列工具應安裝在本機：

+ [JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=limit&amp;p.limit=0&amp;p.limit=144)
+ [Node.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## AEM需求

React應用程式可與下列AEM部署選項搭配使用。 所有部署都需要 [WKND網站v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/tag/aem-guides-wknd-2.1.0) 即將安裝。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 本機設定，使用 [AEM CLOUD SERVICE SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hans)
+ [AEM 6.5 SP13+快速入門](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=zh-Hans？lang=en#install-local-aem-instances)

React應用程式是專為連線至 __AEM發佈__ 不過，如果在React應用程式的設定中提供驗證，則它可以從AEM Author取得內容。

## 使用方式

1. 原地複製 `adobe/aem-guides-wknd-graphql` 存放庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 編輯 `aem-guides-wknd-graphql/react-app/.env.development` 檔案和集合 `REACT_APP_HOST_URI` 指向目標AEM。

   如果連線到作者執行個體，請更新驗證方法。

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

1. 開啟終端機並執行命令：

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. 新瀏覽器視窗應載入於 [http://localhost:3000](http://localhost:3000)
1. 應用程式上應顯示來自WKND參考網站的冒險清單。

## 程式碼

以下是如何建立React應用程式、如何連線到AEM Headless以使用GraphQL持久查詢擷取內容，以及資料如何呈現的摘要。 完整程式碼可在上找到 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/react-app).


### 持久查詢

依照AEM Headless最佳實務，React應用程式會使用AEM GraphQL持續查詢來查詢冒險資料。 應用程式使用兩個持續查詢：

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

每個持續查詢都有對應的React [useEffect](https://reactjs.org/docs/hooks-effect.html) 鉤入 `src/api/usePersistedQueries.js`，非同步呼叫AEM HTTPGET持續查詢端點，並傳回冒險資料。

每個函式又會叫用 `aemHeadlessClient.runPersistedQuery(...)`，執行持續的GraphQL查詢。

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

React應用程式使用兩個檢視，在網頁體驗中呈現冒險資料。

+ `src/components/Adventures.js`

   叫用 `getAdventuresByActivity(..)` 從 `src/api/usePersistedQueries.js` 並在清單中顯示傳回的冒險活動。

+ `src/components/AdventureDetail.js`

   叫用 `getAdventureBySlug(..)` 使用 `slug` 引數是透過 `Adventures` 元件，並顯示單一冒險的詳細資訊。

### 環境變數

數個 [環境變數](https://create-react-app.dev/docs/adding-custom-environment-variables) 用於連線至AEM環境。 預設會連線到執行中的AEM發佈 `http://localhost:4503`. 更新 `.env.development` 檔案中，若要變更AEM連線：

+ `REACT_APP_HOST_URI=http://localhost:4502`：設為AEM目標主機
+ `REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json`：設定GraphQL端點路徑。 此React應用程式不會使用此專案，因為此應用程式只會使用持續性查詢。
+ `REACT_APP_AUTH_METHOD=`：偏好的驗證方法。 選擇性，根據預設不使用驗證。
   + `service-token`：使用服務憑證取得AEMas a Cloud Service上的存取權杖
   + `dev-token`：在AEMas a Cloud Service上使用開發權杖進行本機開發
   + `basic`：透過本機AEM作者使用使用者/通行證進行本機開發
   + 留空可連線到AEM而不進行驗證
+ `REACT_APP_AUTHORIZATION=admin:admin`：設定連線至AEM作者環境時要使用的基本驗證認證（僅供開發）。 如果連線到發佈環境，則不需要此設定。
+ `REACT_APP_DEV_TOKEN`：開發權杖字串。 若要連線到遠端執行個體，您可以在基本驗證(user：pass)旁邊，從雲端主控台使用具有DEV權杖的持有者驗證
+ `REACT_APP_SERVICE_TOKEN`：服務憑證檔案的路徑。 若要連線到遠端執行個體，也可使用服務權杖完成驗證（從開發人員控制檯下載檔案）。

### Proxy AEM要求

使用webpack開發伺服器時(`npm start`)專案需仰賴 [Proxy設定](https://create-react-app.dev/docs/proxying-api-requests-in-development/#configuring-the-proxy-manually) 使用 `http-proxy-middleware`. 檔案設定於 [src/setupProxy.js](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/react-app/src/setupProxy.js) 和仰賴數個在下列位置設定的自訂環境變數： `.env` 和 `.env.development`.

如果連線到AEM作者環境，則對應至 [需要設定驗證方法](#environment-variables).

### 跨原始資源共用(CORS)

此React應用程式仰賴於在目標AEM環境上執行的AEM型CORS設定，並假設React應用程式執行於 `http://localhost:3000` 處於開發模式。 此 [CORS設定](https://github.com/adobe/aem-guides-wknd/blob/main/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config.author/com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql.cfg.json) 是 [WKND網站](https://github.com/adobe/aem-guides-wknd).

![CORS設定](assets/react-app/cross-origin-resource-sharing-configuration.png)
