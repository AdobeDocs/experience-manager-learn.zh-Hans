---
title: 本地开发访问令牌
description: AEM本地开发访问令牌用于加速与AEMas a Cloud Service的集成开发，该集成可通过HTTP以编程方式与AEM创作或发布服务交互。
version: Cloud Service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330477.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2023-01-12T00:00:00Z
exl-id: 197444cb-a68f-4d09-9120-7b6603e1f47d
source-git-commit: 8b6d8d99c806e782a1ddce2b300211f8d4c9da56
workflow-type: tm+mt
source-wordcount: '1067'
ht-degree: 0%

---

# 本地开发访问令牌

构建需要以编程方式访问AEMas a Cloud Service的集成的开发人员需要一种简单、快速的方法来获取AEM的临时访问令牌，以便于进行本地开发活动。 为了满足这一需求，AEM开发人员控制台允许开发人员自行生成临时访问令牌，这些令牌可用于以编程方式访问AEM。

>[!VIDEO](https://video.tv.adobe.com/v/330477/?quality=12&learn=on)

## 生成本地开发访问令牌

![获取本地开发访问令牌](assets/local-development-access-token/getting-a-local-development-access-token.png)

本地开发访问令牌提供了对AEM创作和发布服务的访问权限（作为生成令牌的用户）及其权限。 尽管这是开发令牌，但请勿共享此令牌，或将其存储在源代码管理中。

1. 在 [Adobe Admin Console](https://adminconsole.adobe.com/) 确保您（开发人员）是以下成员：
   + __Cloud Manager — 开发人员__ IMS产品配置文件(授予对AEM开发人员控制台的访问权限)
   + 选择 __AEM管理员__ 或 __AEM用户__ 用于AEM环境服务的IMS产品配置文件访问令牌与
   + 沙盒AEMas a Cloud Service环境只需在 __AEM管理员__ 或 __AEM用户__ 产品配置文件
1. 登录到 [AdobeCloud Manager](https://my.cloudmanager.adobe.com)
1. 打开包含AEMas a Cloud Service环境的程序以与集成
1. 点按 __省略号__ 中环境旁边的 __环境__ ，然后选择 __开发人员控制台__
1. 在中点按 __集成__ 选项卡
1. 点按 __本地令牌__ 选项卡
1. 点按 __获取本地开发令牌__ 按钮
1. 点按 __下载按钮__ 下载包含 `accessToken` ，并将JSON文件保存到开发计算机上的安全位置。
   + 这是您24小时内对AEMas a Cloud Service环境的开发人员访问令牌。

![AEM开发人员控制台 — 集成 — 获取本地开发令牌](./assets/local-development-access-token/developer-console.png)

## 已使用本地开发访问令牌{#use-local-development-access-token}

![本地开发访问令牌 — 外部应用程序](assets/local-development-access-token/local-development-access-token-external-application.png)

1. 从AEM开发人员控制台下载临时的本地开发访问令牌
   + 本地开发访问令牌每24小时过期一次，因此开发人员需要每天下载新的访问令牌
1. 正在开发以编程方式与AEMas a Cloud Service交互的外部应用程序
1. 外部应用程序在本地开发访问令牌中读取
1. 外部应用程序将构建到AEMas a Cloud Service的HTTP请求，并将本地开发访问令牌作为载体令牌添加到HTTP请求的授权标头
1. AEMas a Cloud Service接收HTTP请求、验证请求并执行HTTP请求请求所请求的工作，并将HTTP响应返回到外部应用程序

### 示例外部应用程序

我们将创建一个简单的外部JavaScript应用程序，以说明如何使用本地开发人员访问令牌以编程方式通过HTTPS访问AEMas a Cloud Service。 这说明了 _any_ 在AEM之外运行的应用程序或系统，无论其是框架还是语言，都可以使用访问令牌以编程方式对AEMas a Cloud Service进行身份验证和访问。 在 [下一部分](./service-credentials.md)，我们将更新此应用程序代码，以支持生成用于生产的令牌的方法。

此示例应用程序从命令行运行，并使用以下流程通过AEM Assets HTTP API更新AEM资产元数据：

1. 从命令行中读取参数(`getCommandLineParams()`)
1. 获取用于对AEMas a Cloud Service进行身份验证的访问令牌(`getAccessToken(...)`)
1. 列出在命令行参数中指定的AEM资产文件夹中的所有资产(`listAssetsByFolder(...)`)
1. 使用命令行参数中指定的值更新列出的资产的元数据(`updateMetadata(...)`)

使用访问令牌以编程方式对AEM进行身份验证的关键元素是，将以下格式向对AEM发出的所有HTTP请求添加授权HTTP请求标头：

+ `Authorization: Bearer ACCESS_TOKEN`

## 运行外部应用程序

1. 确保 [Node.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js) 安装在本地开发计算机上，该开发计算机用于运行外部应用程序
1. 下载并解压缩 [示例外部应用程序](./assets/aem-guides_token-authentication-external-application.zip)
1. 从命令行中，在此项目的文件夹中运行 `npm install`
1. 复制 [已下载本地开发访问令牌](#download-local-development-access-token) 到名为 `local_development_token.json` 在项目的根中
   + 但请记住，切勿向Git提交任何凭据！
1. 打开 `index.js` 并查看外部应用程序代码和注释。

   ```javascript
   const fetch = require('node-fetch');
   const fs = require('fs');
   const auth = require('@adobe/jwt-auth');
   
   // The root context of the Assets HTTP API
   const ASSETS_HTTP_API = '/api/assets';
   
   // Command line parameters
   let params = { };
   
   /**
   * Application entry point function
   */
   (async () => {
       console.log('Example usage: node index.js aem=https://author-p1234-e5678.adobeaemcloud.com propertyName=metadata/dc:rights "propertyValue=WKND Limited Use" folder=/wknd-shared/en/adventures/napa-wine-tasting file=credentials-file.json' );
   
       // Parse the command line parameters
       params = getCommandLineParams();
   
       // Set the access token to be used in the HTTP requests to be local development access token
       params.accessToken = await getAccessToken(params.developerConsoleCredentials);
   
       // Get a list of all the assets in the specified assets folder
       let assets = await listAssetsByFolder(params.folder);
   
       // For each asset, update it's metadata
       await assets.forEach(asset => updateMetadata(asset, { 
           [params.propertyName]: params.propertyValue 
       }));
   })();
   
   /**
   * Returns a list of Assets HTTP API asset URLs that reference the assets in the specified folder.
   * 
   * https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=en#retrieve-a-folder-listing
   * 
   * @param {*} folder the Assets HTTP API folder path (less the /content/dam path prefix)
   */
   async function listAssetsByFolder(folder) {
       return fetch(`${params.aem}${ASSETS_HTTP_API}${folder}.json`, {
               method: 'get',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
           })
           .then(res => {
               console.log(`${res.status} - ${res.statusText} @ ${params.aem}${ASSETS_HTTP_API}${folder}.json`);
   
               // If success, return the JSON listing assets, otherwise return empty results
               return res.status === 200 ? res.json() : { entities: [] };
           })
           .then(json => { 
               // Returns a list of all URIs for each non-content fragment asset in the folder
               return json.entities
                   .filter((entity) => entity['class'].indexOf('asset/asset') === -1 && !entity.properties.contentFragment)
                   .map(asset => asset.links.find(link => link.rel.find(r => r === 'self')).href);
           });
   }
   
   /**
   * Update the metadata of an asset in AEM
   * 
   * https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=en#update-asset-metadata
   * 
   * @param {*} asset the Assets HTTP API asset URL to update
   * @param {*} metadata the metadata to update the asset with
   */
   async function updateMetadata(asset, metadata) {        
       await fetch(`${asset}`, {
               method: 'put',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
               body: JSON.stringify({
                   class: 'asset',
                   properties: metadata
               })
           })
           .then(res => { 
               console.log(`${res.status} - ${res.statusText} @ ${asset}`);
           });
   }
   
   /**
   * Parse and return the command line parameters. Expected params are:
   * 
   * - aem = The AEM as a Cloud Service hostname to connect to.
   *              Example: https://author-p12345-e67890.adobeaemcloud.com
   * - folder = The asset folder to update assets in. Note that the Assets HTTP API do NOT use the JCR `/content/dam` path prefix.
   *              Example: '/wknd-shared/en/adventures/napa-wine-tasting'
   * - propertyName = The asset property name to update. Note this is relative to the [dam:Asset]/jcr:content node of the asset.
   *              Example: metadata/dc:rights
   * - propertyValue = The value to update the asset property (specified by propertyName) with.
   *              Example: "WKND Free Use"
   * - file = The path to the JSON file that contains the credentials downloaded from AEM Developer Console
   *              Example: local_development_token_cm_p1234-e5678.json 
   */
   function getCommandLineParams() {
       let parameters = {};
   
       // Parse the command line params, splitting on the = delimiter
       for (let i = 2; i < process.argv.length; i++) {
           let key = process.argv[i].split('=')[0];
           let value = process.argv[i].split('=')[1];
   
           parameters[key] = value;
       };
   
       // Read in the credentials from the provided JSON file
       if (parameters.file) {
           parameters.developerConsoleCredentials = JSON.parse(fs.readFileSync(parameters.file));
       }
   
       console.log(parameters);
   
       return parameters;
   }
   
   async function getAccessToken(developerConsoleCredentials) {s
       if (developerConsoleCredentials.accessToken) {
           // This is a Local Development access token
           return developerConsoleCredentials.accessToken;
       } 
   }
   ```

   查看 `fetch(..)` 在 `listAssetsByFolder(...)` 和 `updateMetadata(...)`，和通知 `headers` 定义 `Authorization` 值为的HTTP请求标头 `Bearer ACCESS_TOKEN`. 这是源自外部应用程序的HTTP请求验证到AEMas a Cloud Service的方式。

   ```javascript
   ...
   return fetch(`${params.aem}${ASSETS_HTTP_API}${folder}.json`, {
               method: 'get',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
   })...
   ```

   对AEMas a Cloud Service的任何HTTP请求都必须在授权标头中设置载体访问令牌。 请记住，每个AEMas a Cloud Service环境都需要其自己的访问令牌。 开发的访问令牌在暂存或生产环境中不起作用，暂存的开发或生产环境中不起作用，而生产环境中的开发或暂存环境则不起作用！

1. 使用命令行，从项目的根执行应用程序，并传递以下参数：

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd-shared/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Limited Use" \
       file=local_development_token.json
   ```

   传入以下参数：

   + `aem`:应用程序与之交互的AEMas a Cloud Service环境的方案和主机名(例如 `https://author-p1234-e5678.adobeaemcloud.com`)。
   + `folder`:其资产更新为的资产文件夹路径 `propertyValue`;请勿添加 `/content/dam` 前缀(例如 `/wknd-shared/en/adventures/napa-wine-tasting`)
   + `propertyName`:要更新的资产属性名称，相对于 `[dam:Asset]/jcr:content` (例如 `metadata/dc:rights`)。
   + `propertyValue`:用于设置 `propertyName` 至；包含空格的值需要使用 `"` (例如 `"WKND Limited Use"`)
   + `file`:从AEM开发人员控制台下载的JSON文件的相对文件路径。

   成功执行每个更新资产的应用程序结果输出：

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### 验证AEM中的元数据更新

通过登录到AEMas a Cloud Service环境，验证元数据是否已更新(确保将相同的主机传递到 `aem` 命令行参数)。

1. 登录外部应用程序与之交互的AEMas a Cloud Service环境(使用 `aem` 命令行参数)
1. 导航到 __资产__ > __文件__
1. 在指定的资产文件夹中导航它 `folder` 命令行参数，例如 __WKND__ > __英语__ > __冒险__ > __纳帕品酒会__
1. 打开 __属性__ （非内容片段）资产
1. 点按 __高级__ 选项卡
1. 例如，查看已更新属性的值 __版权__ 已映射到已更新的 `metadata/dc:rights` JCR属性，该属性反映 `propertyValue` 参数，例如 __WKND有限使用__

![WKND有限使用元数据更新](./assets/local-development-access-token/asset-metadata.png)

## 下面的步骤

现在，我们已使用本地开发令牌以编程方式访问AEMas a Cloud Service。 接下来，我们需要更新应用程序以使用服务凭据来处理，以便此应用程序可以在生产环境中使用。

+ [如何使用服务凭据](./service-credentials.md)
