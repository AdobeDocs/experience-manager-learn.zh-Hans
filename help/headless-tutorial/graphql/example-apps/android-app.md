---
title: Android应用程序 — AEM Headless示例
description: 示例应用程序是探索Adobe Experience Manager (AEM)的Headless功能的绝佳方法。 此Android应用程序演示了如何使用AEM的GraphQL API查询内容。
version: Cloud Service
mini-toc-levels: 2
jira: KT-10588
thumbnail: KT-10588.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headlessas a Cloud Service" before-title="false"
exl-id: 7873e263-b05a-4170-87a9-59e8b7c65faa
duration: 235
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 0%

---

# Android应用程序

示例应用程序是探索Adobe Experience Manager (AEM)的Headless功能的绝佳方法。 此Android应用程序演示了如何使用AEM的GraphQL API查询内容。 此 [适用于Java的AEM Headless客户端](https://github.com/adobe/aem-headless-client-java) 用于执行GraphQL查询并将数据映射到Java对象以向应用程序提供支持。

![带有AEM Headless的Android Java应用程序](./assets/android-java-app/android-app.png)


查看 [GitHub上的源代码](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)

## 前提条件 {#prerequisites}

应在本地安装以下工具：

+ [Android Studio](https://developer.android.com/studio)
+ [Git](https://git-scm.com/)

## AEM要求

Android应用程序可与以下AEM部署选项配合使用。 所有部署都需要 [WKND站点v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) 即将安装。

+ [AEMas a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)

Android应用程序旨在连接到 __AEM发布__ 但是，如果在Android应用程序的配置中提供身份验证，则它可以从AEM Author源内容。

## 使用方法

1. 克隆 `adobe/aem-guides-wknd-graphql` 存储库：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Android Studio](https://developer.android.com/studio) 并打开文件夹 `android-app`
1. 修改文件 `config.properties` 在 `app/src/main/assets/config.properties` 和更新 `contentApi.endpoint` 要匹配您的target AEM环境，请执行以下操作：

   ```plain
   contentApi.endpoint=https://publish-p123-e456.adobeaemcloud.com
   ```

   __基本身份验证__

   此 `contentApi.user` 和 `contentApi.password` 验证有权访问WKND GraphQL内容的本地AEM用户。

   ```plain
   contentApi.endpoint=https://author-p123-e456.adobeaemcloud.com
   contentApi.user=admin
   contentApi.password=admin
   ```

1. 下载 [Android虚拟设备](https://developer.android.com/studio/run/managing-avds) （最低API 28）。
1. 使用Android模拟器构建和部署应用程序。


### 连接到AEM环境

如果连接到AEM创作环境 [授权](https://github.com/adobe/aem-headless-client-java#using-authorization) 为必填项。 此 [AEMHeadlessClientBuilder](https://github.com/adobe/aem-headless-client-java/blob/main/client/src/main/java/com/adobe/aem/graphql/client/AEMHeadlessClientBuilder.java) 提供了使用 [基于令牌的身份验证](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). 要在中使用基于令牌的身份验证更新客户端生成器，请执行以下操作 `AdventureLoader.java` 和 `AdventuresLoader.java`：

```java
/* Comment out basicAuth
 if (user != null && password != null) {
   builder.basicAuth(user, password);
 }
*/

// use token-authentication where `token` is a String representing the token
builder.tokenAuth(token)
```

## 代码

下面简要总结了用于为应用程序提供电源的重要文件和代码。 完整代码可在上找到 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app).

### 持久查询

遵循AEM Headless最佳实践，iOS应用程序使用AEM GraphQL持久查询来查询冒险数据。 该应用程序使用两个持久查询：

+ `wknd/adventures-all` 持久查询，该查询返回在AEM中使用一组删节的属性进行的所有冒险。 此持久查询驱动初始视图的冒险列表。

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
                _dynamicUrl
                _path
                }
            }
        }
    }
}
```

+ `wknd/adventure-by-slug` 持久查询，返回一次冒险 `slug` （唯一标识冒险的自定义属性）和一组完整的属性。 此持久查询为冒险详细信息视图提供支持。

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
          _dynamicUrl
          _path
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

AEM持久查询通过HTTPGET执行，因此， [适用于Java的AEM Headless客户端](https://github.com/adobe/aem-headless-client-java) 用于对AEM执行持久GraphQL查询，并将冒险内容加载到应用程序中。

每个持久查询都有一个相应的“loader”类，该类异步调用AEM HTTPGET端点，并使用自定义的返回冒险数据 [数据模型](#data-models).

+ `loader/AdventuresLoader.java`

  使用获取应用程序主屏幕上的冒险列表 `wknd-shared/adventures-all` 持久查询。

+ `loader/AdventureLoader.java`

  通过获取一个历险来选择它 `slug` 参数，使用 `wknd-shared/adventure-by-slug` 持久查询。

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

### GraphQL响应数据模型{#data-models}

`Adventure.java` 是一个使用GraphQL请求中的JSON数据初始化的Java POJO，它为Android应用程序视图中使用的冒险建模。

### 视图

Android应用程序使用两个视图在移动体验中呈现冒险数据。

+ `AdventureListFragment.java`

  调用 `AdventuresLoader` 并在列表中显示返回的冒险。

+ `AdventureDetailFragment.java`

  调用 `AdventureLoader` 使用 `slug` param通过 `AdventureListFragment` 视图，并显示单次冒险的详细信息。

### 远程图像

`loader/RemoteImagesCache.java` 是一个实用程序类，可帮助在缓存中准备远程图像，以便将其用于Android UI元素。 冒险内容通过URL引用AEM Assets中的图像，此类用于显示该内容。

## 其他资源

+ [AEM Headless快速入门 — GraphQL教程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html?lang=zh-Hans)
+ [适用于Java的AEM Headless客户端](https://github.com/adobe/aem-headless-client-java)
