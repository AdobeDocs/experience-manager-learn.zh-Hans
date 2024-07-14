---
title: 服务器到服务器Node.js应用程序 — AEM Headless示例
description: 示例应用程序是探索Adobe Experience Manager (AEM)的Headless功能的绝佳方法。 此服务器端Node.js应用程序演示了如何通过AEM的GraphQL API，使用持久化查询来查询内容。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-10798
thumbnail: KT-10798.jpg
last-substantial-update: 2023-05-10T00:00:00Z
badgeVersions: label="AEM Headlessas a Cloud Service" before-title="false"
exl-id: 39b21a29-a75f-4a6c-ba82-377cf5cc1726
duration: 135
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---

# 服务器到服务器Node.js应用程序

示例应用程序是探索Adobe Experience Manager (AEM)的Headless功能的绝佳方法。 此服务器到服务器应用程序演示了如何使用AEM的GraphQL API通过持久查询来查询内容并在终端上打印。

使用AEM Headless的![服务器到服务器Node.js应用程序](./assets/server-to-server-app/server-to-server-app.png)

在GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server)上查看[源代码

## 先决条件 {#prerequisites}

应在本地安装以下工具：

+ [Node.js v18](https://nodejs.org/en)
+ [Git](https://git-scm.com/)

## AEM要求

Node.js应用程序可与以下AEM部署选项配合使用。 所有部署都需要安装[WKND站点v3.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest)。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ （可选）如果授权请求(例如，连接到AEM Author服务)，则[服务凭据](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html)。

此Node.js应用程序可根据命令行参数连接到AEM Author或AEM Publish。

## 使用方法

1. 克隆`adobe/aem-guides-wknd-graphql`存储库：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 打开终端并运行以下命令：

   ```shell
   $ cd aem-guides-wknd-graphql/server-to-server-app
   $ npm install
   ```

1. 可以使用命令运行应用程序：

   ```
   $ node index.js <AEM_HOST> <OPTIONAL_SERVICE_CONFIG_FILE_PATH>
   ```

   例如，要未经授权对AEM Publish运行应用程序，请执行以下操作：

   ```shell
   $ node index.js https://publish-p123-e789.adobeaemcloud.com
   ```

   要以授权方式针对AEM Author运行应用程序，请执行以下操作：

   ```shell
   $ node index.js https://author-p123-e456.adobeaemcloud.com ./service-config.json
   ```

1. 终端应打印来自WKND引用站点的JSON冒险列表。

## 代码

以下概要介绍了如何构建服务器到服务器Node.js应用程序，它如何连接到AEM Headless以使用GraphQL持久查询检索内容，以及数据如何呈现。 可在[GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server)上找到完整代码。

服务器到服务器AEM Headless应用程序的常见用例是将AEM中的内容片段数据同步到其他系统，但此应用程序会刻意简化，并会从持久查询中打印JSON结果。

### 持久查询

遵循AEM Headless最佳实践，应用程序使用AEM GraphQL持久查询来查询冒险数据。 该应用程序使用两个持久查询：

+ `wknd/adventures-all`持久查询，该查询返回AEM中的所有冒险，其中具有一组删节的属性。 此持久查询驱动初始视图的冒险列表。

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

### 创建AEM Headless客户端

```javascript
const { AEMHeadless, getToken } = require('@adobe/aem-headless-client-nodejs');

async function run() { 

    // Parse the AEM host, and optional service credentials from the command line arguments
    const args = process.argv.slice(2);
    const aemHost = args.length > 0 ? args[0] : null;                // Example: https://author-p123-e456.adobeaemcloud.com
    const serviceCredentialsFile = args.length > 1 ? args[1] : null; // Example: ./service-config.json

    // If service credentials are provided via command line argument,
    // use `getToken(..)` to exchange them with Adobe IMS for an AEM access token 
    let accessToken;
    if (serviceCredentialsFile) {
        accessToken = (await getToken(serviceCredentialsFile)).accessToken;
    }

    // Instantiate withe AEM Headless client to query AEM GraphQL APIs
    // The endpoint is left blank since only persisted queries should be used to query AEM's GraphQL APIs
    const aemHeadlessClient = new AEMHeadless({
        serviceURL: aemHost,
        endpoint: '',           // Avoid non-persisted queries
        auth: accessToken       // accessToken only set if the 2nd command line parameter is set
    })
    ...
}
```


### 执行GraphQL持久查询

AEM的持久查询通过HTTPGET执行，因此，Node.js的[AEM Headless客户端](https://github.com/adobe/aem-headless-client-nodejs)用于[对AEM执行持久的GraphQL查询](https://github.com/adobe/aem-headless-client-nodejs#within-asyncawait)并检索冒险内容。

通过调用`aemHeadlessClient.runPersistedQuery(...)`并传递持久的GraphQL查询名称来调用持久查询。 GraphQL返回数据后，将其传递到简化的`doSomethingWithDataFromAEM(..)`函数，这将打印结果 — 但通常会将数据发送到另一个系统，或根据检索到的数据生成一些输出。

```js
// index.js

async function run() { 
    ...
    try {
        // Retrieve the data from AEM GraphQL APIs
        data = await aemHeadlessClient.runPersistedQuery('wknd-shared/adventures-all')
        
        // Do something with the data from AEM. 
        // A common use case is sending the data to another system.
        await doSomethingWithDataFromAEM(data);
    } catch (e) {
        console.error(e.toJSON())
    }
}
```
