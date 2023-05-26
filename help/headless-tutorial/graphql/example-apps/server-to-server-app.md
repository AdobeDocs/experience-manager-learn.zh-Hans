---
title: 服务器到服务器Node.js应用程序 — AEM Headless示例
description: 示例应用程序是探索Adobe Experience Manager (AEM)的Headless功能的绝佳方法。 此服务器端Node.js应用程序演示了如何使用AEM GraphQL API通过持久查询来查询内容。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 10798
thumbnail: KT-10798.jpg
exl-id: 39b21a29-a75f-4a6c-ba82-377cf5cc1726
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 4%

---

# 服务器到服务器Node.js应用程序

示例应用程序是探索Adobe Experience Manager (AEM)的Headless功能的绝佳方法。 此服务器到服务器应用程序演示了如何使用AEM GraphQL API通过持久查询来查询内容，并在终端上打印该内容。

![带有AEM Headless的服务器到服务器Node.js应用程序](./assets/server-to-server-app/server-to-server-app.png)

查看 [GitHub上的源代码](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server-app)

## 前提条件 {#prerequisites}

应在本地安装以下工具：

+ [Node.js v18](https://nodejs.org/en/)
+ [Git](https://git-scm.com/)

## AEM要求

Node.js应用程序可与以下AEM部署选项配合使用。 所有部署都需要 [WKND站点v2.0.0+](https://github.com/adobe/aem-guides-wknd/releases/latest) 即将安装。

+ [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/overview.html)
+ （可选） [服务凭据](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html) 如果授权请求（例如，连接到AEM作者服务）。

此Node.js应用程序可以根据命令行参数连接到AEM创作或AEM发布。

## 使用方法

1. 克隆 `adobe/aem-guides-wknd-graphql` 存储库：

   ```shell
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. 打开终端并运行以下命令：

   ```shell
   $ cd aem-guides-wknd-graphql/server-to-server-app
   $ npm install
   ```

1. 可以使用以下命令运行应用程序：

   ```
   $ node index.js <AEM_HOST> <OPTIONAL_SERVICE_CONFIG_FILE_PATH>
   ```

   例如，要对AEM Publish运行应用程序而不授权，请执行以下操作：

   ```shell
   $ node index.js https://publish-p123-e789.adobeaemcloud.com
   ```

   要通过授权对AEM作者运行应用程序，请执行以下操作：

   ```shell
   $ node index.js https://author-p123-e456.adobeaemcloud.com ./service-config.json
   ```

1. 终端应打印来自WKND引用站点的JSON冒险列表。

## 代码

以下摘要介绍了如何构建服务器到服务器Node.js应用程序，它如何连接到AEM Headless以使用GraphQL持久查询检索内容，以及数据如何呈现。 完整代码可在上找到 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/server-to-server-app).

服务器到服务器AEM Headless应用程序的常见用例是将内容片段数据从AEM同步到其他系统，但此应用程序有意地简单，并会打印来自持久查询的JSON结果。

### 持久查询

遵循AEM Headless最佳实践，应用程序使用AEM GraphQL持久查询来查询冒险数据。 应用程序使用两个持久查询：

+ `wknd/adventures-all` 持久查询，该查询返回AEM中的所有冒险以及一组删节的资产。 此持久查询驱动初始视图的冒险列表。

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

AEM持久查询通过HTTPGET执行，因此， [适用于Node.js的AEM Headless客户端](https://github.com/adobe/aem-headless-client-nodejs) 已用于 [执行持久的GraphQL查询](https://github.com/adobe/aem-headless-client-nodejs#within-asyncawait) 针对AEM并检索冒险内容。

通过调用调用持久查询 `aemHeadlessClient.runPersistedQuery(...)`，并传递持久的GraphQL查询名称。 GraphQL返回数据后，将其传递到简化的 `doSomethingWithDataFromAEM(..)` 函数，可打印结果 — 但通常会将数据发送到另一个系统，或根据检索到的数据生成一些输出。

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
