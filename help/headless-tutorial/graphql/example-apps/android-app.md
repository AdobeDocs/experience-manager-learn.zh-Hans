---
title: Android应用程序 — AEM Headless示例
description: 示例应用程序是探索Adobe Experience Manager(AEM)无头功能的绝佳方式。 提供了一个Android应用程序，用于演示如何使用AEM的GraphQL API查询内容。 Apollo客户端Android用于生成GraphQL查询，并将数据映射到Swift对象以为应用程序提供动力。 SwiftUI用于呈现内容的简单列表和详细视图。
version: Cloud Service
mini-toc-levels: 1
kt: 9166
thumbnail: KT-9166.jpg
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: 9b1e38c8d4a0301c124c6f1607a9e4362b0e9cd1
workflow-type: tm+mt
source-wordcount: '734'
ht-degree: 4%

---


# Android应用程序

示例应用程序是探索Adobe Experience Manager(AEM)无头功能的绝佳方式。 提供了一个Android应用程序，用于演示如何使用AEM的GraphQL API查询内容。 的 [AEM Headless Client for Java](https://github.com/adobe/aem-headless-client-java) 用于执行GraphQL查询并将数据映射到Java对象以为应用程序提供动力。

查看 [GitHub上的源代码](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)

>[!VIDEO](https://video.tv.adobe.com/v/338093/?quality=12&learn=on)

## 前提条件 {#prerequisites}

应在本地安装以下工具：

* [Android Studio](https://developer.android.com/studio)
* [Git](https://git-scm.com/)

## AEM要求

应用程序旨在连接到AEM **发布** 的 [WKND参考站点](https://github.com/adobe/aem-guides-wknd/releases/latest) 已安装。

* [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/overview/introduction.html)
* [AEM 6.5.10+](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/service-pack/new-features-latest-service-pack.html?lang=zh-Hans)

我们建议 [将WKND引用站点部署到Cloud Service环境](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html#coding-against-the-right-aem-version). 使用的本地设置 [AEM Cloud Service SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html) 或 [AEM 6.5快速入门Jar](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=en#install-local-aem-instances) 也可使用。

## 使用方法

1. 克隆 `aem-guides-wknd-graphql` 存储库：

   ```shell
   git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Launch [Android Studio](https://developer.android.com/studio) 并打开文件夹 `android-app`
1. 修改文件 `config.properties` at `app/src/main/assets/config.properties` 更新 `contentApi.endpoint` 要匹配target AEM环境，请执行以下操作：

   ```plain
   contentApi.endpoint=http://10.0.2.2:4502
   contentApi.user=admin
   contentApi.password=admin
   ```

1. 下载 [Android虚拟设备](https://developer.android.com/studio/run/managing-avds) （最低API 28）
1. 使用Android模拟器构建和部署应用程序。


### 连接到AEM环境

`10.0.2.2` 是 [特殊别名](https://developer.android.com/studio/run/emulator-networking) 用于本地主机。 所以 `10.0.2.2:4502` 等于 `localhost:4502`. 如果连接到AEM发布环境（推荐），则无需授权，并且 `contentAPi.user` 和 `contentApi.password` 可留空。

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

### 获取内容

的 [AEM Headless Client for Java](https://github.com/adobe/aem-headless-client-java) 应用程序使用来对AEM执行GraphQL查询，并将冒险内容加载到应用程序中。

`AdventuresLoader.java` 是在应用程序主屏幕上获取并加载Adventures初始列表的文件。 它利用 [持久化查询](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/video-series/graphql-persisted-queries.html) 其中 [预包装](https://github.com/adobe/aem-guides-wknd/tree/master/ui.content/src/main/content/jcr_root/conf/wknd/settings/graphql/persistentQueries/adventures-all/_jcr_content) 和WKND引用站点。 端点为 `/wknd/adventures-all`. `AEMHeadlessClientBuilder` 基于中设置的api端点实例化新实例 `config.properties`.

```java
//AdventuresLoader.java

public static final String PERSISTED_QUERY_NAME = "/wknd/adventures-all";
...
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());
// optional authentication for basic auth
String user = config.getContentApiUser();
String password = config.getContentApiPassword();
if (user != null && password != null) {
    builder.basicAuth(user, password);
}

AEMHeadlessClient client = builder.build();
// run a persistent query and get a response
GraphQlResponse response = client.runPersistedQuery(PERSISTED_QUERY_NAME);
```

`AdventureLoader.java` 是为每个详细视图获取并加载Adventure内容的文件。 再次 `AEMHeadlessClient` 用于执行查询。 根据Adventure内容片段的路径执行常规graphQL查询。 查询将在名为的单独文件中进行维护 [adventureByPath.query](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/android-app/app/src/main/assets/adventureByPath.query)

```java
AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(config.getContentApiEndpoint());
String user = config.getContentApiUser();
String password = config.getContentApiPassword();
if (user != null && password != null) {
    builder.basicAuth(user, password);
}
AEMHeadlessClient client = builder.build();

// based on the file adventureByPath.query
String query = readFile(getContext(), QUERY_FILE_NAME);

// construct a parameter map to dynamically pass in adventure path
Map<String, Object> params = new HashMap<>();
params.put("adventurePath", this.path);

// execute the query based on the adventureByPath query and passed in parameters
GraphQlResponse response = client.runQuery(query, params);
```

`Adventure.java` 是一个POJO，通过GraphQL请求中的JSON数据进行初始化。

`RemoteImagesCache.java` 是一个实用程序类，可帮助在缓存中准备远程图像，以便与Android UI元素一起使用。 Adventure内容通过URL引用AEM Assets中的图像，此类用于显示该内容。

### 视图

`AdventureListFragment.java`  — 在调用时触发 `AdventuresLoader` 并在列表中显示返回的冒险。

`AdventureDetailFragment.java`  — 初始化 `AdventureLoader` 并显示单次冒险的细节。

## 其他资源

* [AEM Headless入门 — GraphQL教程](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview.html)
* [AEM Headless Client for Java](https://github.com/adobe/aem-headless-client-java)

