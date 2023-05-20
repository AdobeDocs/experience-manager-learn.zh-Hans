---
title: Android應用程式 — AEM Headless範例
description: 範例應用程式是探索Adobe Experience Manager (AEM)的Headless功能的絕佳方式。 此Android應用程式示範了如何使用AEM的GraphQL API來查詢內容。
version: Cloud Service
mini-toc-levels: 2
kt: 10588
thumbnail: KT-10588.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 7873e263-b05a-4170-87a9-59e8b7c65faa
source-git-commit: 985d52f02025dc9cb2b9c70ead4a88af07c63f29
workflow-type: tm+mt
source-wordcount: '765'
ht-degree: 6%

---

# Android應用程式

範例應用程式是探索Adobe Experience Manager (AEM)的Headless功能的絕佳方式。 此Android應用程式示範了如何使用AEM的GraphQL API來查詢內容。 此 [適用於Java的AEM Headless使用者端](https://github.com/adobe/aem-headless-client-java) 用於執行GraphQL查詢，並將資料對應至Java物件以支援應用程式。

![使用AEM Headless的Android Java應用程式](./assets/android-java-app/android-app.png)


檢視 [GitHub上的原始程式碼](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)

## 前提条件 {#prerequisites}

下列工具應安裝在本機：

+ [Android Studio](https://developer.android.com/studio)
+ [Git](https://git-scm.com/)

## AEM需求

Android應用程式可與下列AEM部署選項搭配使用。 所有部署都需要 [WKND網站v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) 即將安裝。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 本機設定，使用 [AEM CLOUD SERVICE SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=zh-Hans)
+ [AEM 6.5 SP13+快速入門](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=zh-Hans？lang=en#install-local-aem-instances)

Android應用程式是專為連線至 __AEM發佈__ 不過，如果在Android應用程式的設定中提供驗證，則它可以從AEM Author取得內容。

## 使用方式

1. 原地複製 `adobe/aem-guides-wknd-graphql` 存放庫：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Android Studio](https://developer.android.com/studio) 並開啟資料夾 `android-app`
1. 修改檔案 `config.properties` 於 `app/src/main/assets/config.properties` 和更新 `contentApi.endpoint` 若要符合您的目標AEM環境：

   ```plain
   contentApi.endpoint=http://10.0.2.2:4503
   ```

   __基本驗證__

   此 `contentApi.user` 和 `contentApi.password` 驗證可存取WKND GraphQL內容的本機AEM使用者。

   ```plain
   contentApi.endpoint=http://10.0.2.2:4502
   contentApi.user=admin
   contentApi.password=admin
   ```

1. 下載 [Android虛擬裝置](https://developer.android.com/studio/run/managing-avds) （最低API 28）。
1. 使用Android模擬器建置和部署應用程式。


### 連線到AEM環境

`10.0.2.2` 是 [特殊別名IP](https://developer.android.com/studio/run/emulator-networking) 適用於localhost （使用模擬器製作時） `10.0.2.2:4502` 相當於 `localhost:4502`. 如果連線到AEM發佈環境（建議），則不需要授權，並且 `contentAPi.user` 和 `contentApi.password` 可保留空白。

如果連線到AEM作者環境 [授權](https://github.com/adobe/aem-headless-client-java#using-authorization) 為必填欄位。 依預設，應用程式設定為使用基本驗證，使用者名稱和密碼為 `admin:admin`. 此 [AEMHeadlessClientBuilder](https://github.com/adobe/aem-headless-client-java/blob/main/client/src/main/java/com/adobe/aem/graphql/client/AEMHeadlessClientBuilder.java) 提供使用 [權杖型驗證](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). 若要在中使用權杖型驗證更新使用者端產生器 `AdventureLoader.java` 和 `AdventuresLoader.java`：

```java
/* Comment out basicAuth
 if (user != null && password != null) {
   builder.basicAuth(user, password);
 }
*/

// use token-authentication where `token` is a String representing the token
builder.tokenAuth(token)
```

## 程式碼

以下是用來支援應用程式的重要檔案和程式碼摘要。 完整程式碼可在上找到 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app).

### 持久查詢

依照AEM Headless最佳實務，iOS應用程式會使用AEM GraphQL持續查詢來查詢冒險資料。 應用程式使用兩個持續查詢：

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

AEM持續查詢會透過HTTPGET執行，因此， [適用於Java的AEM Headless使用者端](https://github.com/adobe/aem-headless-client-java) 用於對AEM執行持續的GraphQL查詢，並將冒險內容載入應用程式。

每個持續查詢都有對應的「loader」類別，可非同步呼叫AEM HTTPGET端點，並使用自訂定義傳回冒險資料 [資料模型](#data-models).

+ `loader/AdventuresLoader.java`

   使用在應用程式的主畫面上擷取冒險清單 `wknd-shared/adventures-all` 持久查詢。

+ `loader/AdventureLoader.java`

   透過以下方式擷取單一冒險選項： `slug` 引數，使用 `wknd-shared/adventure-by-slug` 持久查詢。

```java
//AdventuresLoader.java

public static final String PERSISTED_QUERY_NAME = "/wknd-shared/adventures-all";
...
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());

// Optional authentication for basic auth
String user = config.getContentApiUser();
String password = config.getContentApiPassword();

if (user != null && password != null) {
    builder.basicAuth(user, password);
}

AEMHeadlessClient client = builder.build();
// run a persistent query and get a response
GraphQlResponse response = client.runPersistedQuery(PERSISTED_QUERY_NAME);
```

### GraphQL回應資料模型{#data-models}

`Adventure.java` 是一種Java POJO，會使用GraphQL請求中的JSON資料初始化，並為冒險建模，以用於Android應用程式的檢視。

### 视图

Android應用程式使用兩個檢視，在行動體驗中呈現冒險資料。

+ `AdventureListFragment.java`

   叫用 `AdventuresLoader` 並在清單中顯示傳回的冒險活動。

+ `AdventureDetailFragment.java`

   叫用 `AdventureLoader` 使用 `slug` 引數是透過 `AdventureListFragment` 檢視，並顯示單一冒險的詳細資訊。

### 遠端影像

`loader/RemoteImagesCache.java` 是一個公用程式類別，可協助您在快取中準備遠端影像，以便與Android UI元素搭配使用。 冒險內容會透過URL參考AEM Assets中的影像，此類別用於顯示該內容。

## 其他資源

+ [AEM Headless快速入門 — GraphQL教學課程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=zh-Hans)
+ [適用於Java的AEM Headless使用者端](https://github.com/adobe/aem-headless-client-java)
