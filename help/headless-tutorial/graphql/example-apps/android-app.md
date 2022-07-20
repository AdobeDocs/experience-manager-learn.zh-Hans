---
title: Android应用程序 — AEM Headless示例
description: 示例应用程序是探索Adobe Experience Manager(AEM)无头功能的绝佳方式。 此Android应用程序演示了如何使用AEM的GraphQL API查询内容。
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
ht-degree: 3%

---

# Android应用程序

示例应用程序是探索Adobe Experience Manager(AEM)无头功能的绝佳方式。 此Android应用程序演示了如何使用AEM的GraphQL API查询内容。 的 [AEM Headless Client for Java](https://github.com/adobe/aem-headless-client-java) 用于执行GraphQL查询并将数据映射到Java对象以为应用程序提供动力。

![使用AEM Headless的Android Java应用程序](./assets/android-java-app/android-app.png)


查看 [GitHub上的源代码](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)

## 前提条件 {#prerequisites}

应在本地安装以下工具：

+ [Android Studio](https://developer.android.com/studio)
+ [Git](https://git-scm.com/)

## AEM要求

Android应用程序可与以下AEM部署选项配合使用。 所有部署都需要 [WKND Site v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) 安装。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ 使用进行本地设置 [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)
+ [AEM 6.5 SP13+快速入门](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances)

Android应用程序旨在连接到 __AEM发布__ 环境中，但是，如果在Android应用程序配置中提供了身份验证，则可以从AEM创作中源内容。

## 使用方法

1. 克隆 `adobe/aem-guides-wknd-graphql` 存储库：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Android Studio](https://developer.android.com/studio) 并打开文件夹 `android-app`
1. 修改文件 `config.properties` at `app/src/main/assets/config.properties` 更新 `contentApi.endpoint` 要匹配target AEM环境，请执行以下操作：

   ```plain
   contentApi.endpoint=http://10.0.2.2:4503
   ```

   __基本身份验证__

   的 `contentApi.user` 和 `contentApi.password` 验证本地AEM用户对WKND GraphQL内容的访问权限。

   ```plain
   contentApi.endpoint=http://10.0.2.2:4502
   contentApi.user=admin
   contentApi.password=admin
   ```

1. 下载 [Android虚拟设备](https://developer.android.com/studio/run/managing-avds) （最低API为28）。
1. 使用Android模拟器构建和部署应用程序。


### 连接到AEM环境

`10.0.2.2` 是 [特殊别名IP](https://developer.android.com/studio/run/emulator-networking) 在使用模拟器进行 `10.0.2.2:4502` 等于 `localhost:4502`. 如果连接到AEM发布环境（推荐），则无需授权，并且 `contentAPi.user` 和 `contentApi.password` 可留空。

如果连接到AEM创作环境 [授权](https://github.com/adobe/aem-headless-client-java#using-authorization) 必需。 默认情况下，应用程序设置为使用基本身份验证，并且用户名和密码为 `admin:admin`. 的 [AEMHeadlessClientBuilder](https://github.com/adobe/aem-headless-client-java/blob/main/client/src/main/java/com/adobe/aem/graphql/client/AEMHeadlessClientBuilder.java) 提供使用 [基于令牌的身份验证](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). 在中使用基于令牌的身份验证更新客户端生成器 `AdventureLoader.java` 和 `AdventuresLoader.java`:

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

以下是用于为应用程序提供支持的重要文件和代码的简短摘要。 完整代码可在 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app).

### 持久化查询

遵循AEM无头最佳实践，iOS应用程序使用AEM GraphQL持久查询来查询冒险数据。 应用程序使用两个持久查询：

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

AEM持久查询是通过HTTPGET执行的，因此， [AEM Headless Client for Java](https://github.com/adobe/aem-headless-client-java) 用于对AEM执行持久的GraphQL查询，并将冒险内容加载到应用程序中。

每个持久化查询都有一个相应的“loader”类，该类异步调用AEM HTTPGET端点，并使用定义的自定义返回冒险数据 [数据模型](#data-models).

+ `loader/AdventuresLoader.java`

   使用获取应用程序主屏幕上的冒险列表 `wknd-shared/adventures-all` 保留查询。

+ `loader/AdventureLoader.java`

   通过选择单个冒险 `slug` 参数，使用 `wknd-shared/adventure-by-slug` 保留查询。

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

`Adventure.java` 是一个Java POJO，通过GraphQL请求中的JSON数据初始化，并为在Android应用程序视图中使用的冒险活动建模。

### 视图

Android应用程序使用两个视图来显示移动体验中的冒险数据。

+ `AdventureListFragment.java`

   调用 `AdventuresLoader` 并在列表中显示返回的冒险。

+ `AdventureDetailFragment.java`

   调用 `AdventureLoader` 使用 `slug` 通过 `AdventureListFragment` 查看，并显示单个冒险的详细信息。

### 远程映像

`loader/RemoteImagesCache.java` 是一个实用程序类，可帮助在缓存中准备远程图像，以便与Android UI元素一起使用。 冒险内容通过URL引用AEM Assets中的图像，此类用于显示该内容。

## 其他资源

+ [AEM Headless入门 — GraphQL教程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
+ [AEM Headless Client for Java](https://github.com/adobe/aem-headless-client-java)
