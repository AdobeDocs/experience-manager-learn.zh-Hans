---
title: 本地开发访问令牌
description: AEM本地开发访问令牌用于加快与AEMas a Cloud Service的集成开发，以便通过HTTP以编程方式与AEM创作或发布服务交互。
version: Cloud Service
feature: APIs
jira: KT-6785
thumbnail: 330477.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2023-01-12T00:00:00Z
doc-type: Tutorial
exl-id: 197444cb-a68f-4d09-9120-7b6603e1f47d
duration: 715
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1067'
ht-degree: 0%

---

# 本地开发访问令牌

在构建集成时，如果开发人员需要以编程方式访问AEMas a Cloud Service，则需要一种简单、快速的方式获取AEM的临时访问令牌，以便利本地开发活动。 为了满足此需求，AEM Developer Console允许开发人员自行生成可用于以编程方式访问AEM的临时访问令牌。

>[!VIDEO](https://video.tv.adobe.com/v/330477?quality=12&learn=on)

## 生成本地开发访问令牌

![获取本地开发访问令牌](assets/local-development-access-token/getting-a-local-development-access-token.png)

本地开发访问令牌允许作为生成令牌的用户访问AEM创作和发布服务，以及这些服务的权限。 尽管这是一个开发令牌，但请不要共享此令牌，或将其存储在源代码管理中。

1. 在 [Adobe Admin Console](https://adminconsole.adobe.com/) 确保您（开发人员）是以下成员之一：
   + __Cloud Manager — 开发人员__ IMS产品配置文件(授予对AEM开发人员控制台的访问权限)
   + 或者是 __AEM管理员__ 或 __AEM用户__ 访问令牌与集成的AEM环境服务的IMS产品配置文件
   + 沙盒AEMas a Cloud Service环境只需要以下任一成员资格 __AEM管理员__ 或 __AEM用户__ 产品配置文件
1. 登录 [AdobeCloud Manager](https://my.cloudmanager.adobe.com)
1. 打开包含要与集成的AEMas a Cloud Service环境的程序
1. 点按 __省略号__ 中的环境旁边 __环境__ 部分，然后选择 __开发人员控制台__
1. 点击 __集成__ 选项卡
1. 点按 __本地令牌__ 选项卡
1. 点按 __获取本地开发令牌__ 按钮
1. 点击 __下载按钮__ ，以下载包含的JSON文件 `accessToken` 并将JSON文件保存到开发计算机上的安全位置。
   + 这是您的24小时开发人员访问AEMas a Cloud Service环境的令牌。

![AEM开发人员控制台 — 集成 — 获取本地开发令牌](./assets/local-development-access-token/developer-console.png)

## 已使用本地开发访问令牌{#use-local-development-access-token}

![本地开发访问令牌 — 外部应用程序](assets/local-development-access-token/local-development-access-token-external-application.png)

1. 从AEM开发人员控制台下载临时本地开发访问令牌
   + 本地开发访问令牌每24小时过期一次，因此开发人员需要每天下载新的访问令牌
1. 正在开发一个以编程方式与AEMas a Cloud Service交互的外部应用程序
1. 外部应用程序读取本地开发访问令牌
1. 外部应用程序构造对AEMas a Cloud Service的HTTP请求，将本地开发访问令牌作为持有者令牌添加到HTTP请求的授权标头
1. AEMas a Cloud Service接收HTTP请求，对请求进行身份验证，并执行HTTP请求所请求的工作，并将HTTP响应返回给外部应用程序

### 示例外部应用程序

我们将创建一个简单的外部JavaScript应用程序，以说明如何使用本地开发人员访问令牌以编程方式通过HTTPS访问AEMas a Cloud Service。 这说明了如何 _任意_ 在AEM之外运行的应用程序或系统，无论框架或语言如何，都可以使用访问令牌以编程方式向AEMas a Cloud Service进行身份验证和访问。 在 [下一节](./service-credentials.md)中，我们将更新此应用程序代码以支持用于生成生产用令牌的方法。

此示例应用程序从命令行运行，并使用AEM Assets HTTP API通过以下流程更新AEM资源元数据：

1. 从命令行读取参数(`getCommandLineParams()`)
1. 获取用于向AEMas a Cloud Service进行身份验证的访问令牌(`getAccessToken(...)`)
1. 列出在命令行参数中指定的AEM asset文件夹中的所有资源(`listAssetsByFolder(...)`)
1. 使用命令行参数(`updateMetadata(...)`)

使用访问令牌以编程方式向AEM进行身份验证的关键元素是，按照以下格式向向AEM发出的所有HTTP请求添加授权HTTP请求标头：

+ `Authorization: Bearer ACCESS_TOKEN`

## 运行外部应用程序

1. 确保 [Node.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js) 安装在本地开发计算机上，用于运行外部应用程序
1. 下载并解压缩 [示例外部应用程序](./assets/aem-guides_token-authentication-external-application.zip)
1. 从命令行中，在此项目的文件夹中运行 `npm install`
1. 复制 [已下载本地开发访问令牌](#download-local-development-access-token) 到名为的文件 `local_development_token.json` 在项目的根目录下
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

   查看 `fetch(..)` 中的调用 `listAssetsByFolder(...)` 和 `updateMetadata(...)`，并注意 `headers` 定义 `Authorization` 值为的HTTP请求头 `Bearer ACCESS_TOKEN`. 这是源自外部应用程序的HTTP请求向AEMas a Cloud Service进行身份验证的方式。

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

   对AEM发出任何HTTP请求as a Cloud Service，必须在授权标头中设置持有者访问令牌。 请记住，每个AEMas a Cloud Service环境都需要其自身的访问令牌。 开发的访问令牌不适用于暂存或生产，暂存的令牌不适用于开发或生产，生产的令牌不适用于开发或暂存！

1. 使用命令行，从项目的根目录执行应用程序，传入以下参数：

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd-shared/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Limited Use" \
       file=local_development_token.json
   ```

   以下参数在中传递：

   + `aem`：应用程序与之交互的AEMas a Cloud Service环境的方案和主机名(例如， `https://author-p1234-e5678.adobeaemcloud.com`)。
   + `folder`：其资源已使用更新的资源文件夹路径 `propertyValue`；请勿添加 `/content/dam` 前缀(例如 `/wknd-shared/en/adventures/napa-wine-tasting`)
   + `propertyName`：要更新的资产属性名称，相对于 `[dam:Asset]/jcr:content` (例如： `metadata/dc:rights`)。
   + `propertyValue`：用于设置 `propertyName` 到；带有空格的值需要封装为 `"` (例如： `"WKND Limited Use"`)
   + `file`：从AEM开发人员控制台下载的JSON文件的相对文件路径。

   成功执行每个资源的应用程序结果输出已更新：

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### 在AEM中验证元数据更新

通过登录到AEMas a Cloud Service环境，验证元数据是否已更新(确保将相同的主机传递到 `aem` 命令行参数)。

1. 登录到外部应用程序与之交互的AEMas a Cloud Service环境（使用中提供的相同主机） `aem` 命令行参数)
1. 导航至 __资产__ > __文件__
1. 导航到指定的资源文件夹。 `folder` 命令行参数，例如 __WKND__ > __英语__ > __冒险__ > __纳帕品酒会__
1. 打开 __属性__ 文件夹中的任何（非内容片段）资产
1. 点按至 __高级__ 选项卡
1. 查看已更新属性的值，例如 __版权__ 映射到已更新的 `metadata/dc:rights` JCR属性，它反映了 `propertyValue` 参数，例如 __WKND有限使用__

![WKND有限使用元数据更新](./assets/local-development-access-token/asset-metadata.png)

## 后续步骤

现在，我们使用本地开发令牌以编程方式访问了AEMas a Cloud Service。 接下来，我们需要更新应用程序以使用服务凭据进行处理，以便在生产上下文中使用此应用程序。

+ [如何使用服务凭据](./service-credentials.md)
