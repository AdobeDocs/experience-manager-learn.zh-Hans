---
title: 本地开发访问令牌
description: AEM本地开发访问令牌用于加快与AEM的集成开发，作为一个Cloud Service，可通过HTTP以编程方式与AEM创作或发布服务进行交互。
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
exl-id: 197444cb-a68f-4d09-9120-7b6603e1f47d
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '1070'
ht-degree: 0%

---

# 本地开发访问令牌

构建集成(需要以编程方式访问AEM作为Cloud Service)的开发人员需要一种简单、快速的方法来获取AEM的临时访问令牌，以便于进行本地开发活动。 为了满足这一需求，AEM开发人员控制台允许开发人员自行生成临时访问令牌，这些令牌可用于以编程方式访问AEM。

>[!VIDEO](https://video.tv.adobe.com/v/330477/?quality=12&learn=on)

## 生成本地开发访问令牌

![获取本地开发访问令牌](assets/local-development-access-token/getting-a-local-development-access-token.png)

本地开发访问令牌提供了对AEM创作和发布服务的访问权限（作为生成令牌的用户）及其权限。 尽管这是开发令牌，但请勿共享此令牌，或将其存储在源代码管理中。

1. 在[AdobeAdminConsole](https://adminconsole.adobe.com/)中，确保您（开发人员）是以下成员：
   + __Cloud Manager - DeveloperIMS__ 产品配置文件(授予对AEM开发人员控制台的访问权限)
   + __AEM管理员__&#x200B;或&#x200B;__AEM用户__ AEM环境服务的IMS产品配置文件访问令牌将与集成
   + 沙盒AEM作为Cloud Service环境，只需要在&#x200B;__AEM Administrators__&#x200B;或&#x200B;__AEM Users__&#x200B;产品配置文件中具有成员资格即可
1. 登录到[AdobeCloud Manager](https://my.cloudmanager.adobe.com)
1. 打开包含AEM作为Cloud Service环境的程序以与集成
1. 点按&#x200B;__环境__&#x200B;部分中环境旁边的&#x200B;__省略号__ ，然后选择&#x200B;__开发人员控制台__
1. 点按&#x200B;__集成__&#x200B;选项卡中的
1. 点按&#x200B;__获取本地开发令牌__&#x200B;按钮
1. 点按左上角的&#x200B;__下载按钮__&#x200B;以下载包含`accessToken`值的JSON文件，然后将JSON文件保存到开发计算机上的安全位置。
   + 这是您24小时的开发人员访问令牌，用于将AEM作为Cloud Service环境。

![AEM开发人员控制台 — 集成 — 获取本地开发令牌](./assets/local-development-access-token/developer-console.png)

## 已使用本地开发访问令牌{#use-local-development-access-token}

![本地开发访问令牌 — 外部应用程序](assets/local-development-access-token/local-development-access-token-external-application.png)

1. 从AEM开发人员控制台下载临时的本地开发访问令牌
   + 本地开发访问令牌每24小时过期一次，因此开发人员将需要每天下载新的访问令牌
1. 正在开发以编程方式与AEM作为Cloud Service交互的外部应用程序
1. 外部应用程序在本地开发访问令牌中读取
1. 外部应用程序将HTTP请求构建为AEM作为Cloud Service，并将本地开发访问令牌作为载体令牌添加到HTTP请求的授权标头
1. AEM as a A Service接收HTTP请求、验证请求并执行HTTP请求请求所请求的工作，并将HTTP响应返回给外部应用程序

### 示例外部应用程序

我们将创建一个简单的外部JavaScript应用程序，以说明如何使用本地开发人员访问令牌以编程方式通过HTTPS访问AEM as aCloud Service。 这说明了&#x200B;_任何在AEM外运行的_&#x200B;应用程序或系统，无论其框架或语言如何，都可以使用访问令牌以编程方式对AEM进行身份验证并将其作为Cloud Service进行访问。 在[下一部分](./service-credentials.md)中，我们将更新此应用程序代码，以支持生成生产用令牌的方法。

此示例应用程序从命令行运行，并使用以下流程通过AEM Assets HTTP API更新AEM资产元数据：

1. 从命令行中读取参数(`getCommandLineParams()`)
1. 获取用于对AEM作为Cloud Service进行身份验证的访问令牌(`getAccessToken(...)`)
1. 列出在命令行参数(`listAssetsByFolder(...)`)中指定的AEM资产文件夹中的所有资产
1. 使用命令行参数(`updateMetadata(...)`)中指定的值更新列出的资产元数据

使用访问令牌以编程方式对AEM进行身份验证的关键元素是，将以下格式向对AEM发出的所有HTTP请求添加授权HTTP请求标头：

+ `Authorization: Bearer ACCESS_TOKEN`

## 运行外部应用程序

1. 确保在本地开发计算机上安装了[Node.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js)，该开发计算机将用于运行外部应用程序
1. 下载并解压缩[示例外部应用程序](./assets/aem-guides_token-authentication-external-application.zip)
1. 在此项目文件夹的命令行中，运行`npm install`
1. 将[下载的本地开发访问令牌](#download-local-development-access-token)复制到项目根目录中名为`local_development_token.json`的文件中
   + 但请记住，切勿向Git提交任何凭据！
1. 打开`index.js`并查看外部应用程序代码和注释。

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
       console.log('Example usage: node index.js aem=https://author-p1234-e5678.adobeaemcloud.com propertyName=metadata/dc:rights "propertyValue=WKND Limited Use" folder=/wknd/en/adventures/napa-wine-tasting file=credentials-file.json' );
   
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
   *              Example: '/wknd/en/adventures/napa-wine-tasting'
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

   查看`listAssetsByFolder(...)`和`updateMetadata(...)`中的`fetch(..)`调用，并注意`headers`定义值为`Bearer ACCESS_TOKEN`的`Authorization` HTTP请求标头。 这是源自外部应用程序的HTTP请求作为Cloud Service验证到AEM的方式。

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

   对AEM作为Cloud Service的任何HTTP请求，都必须在授权标头中设置载体访问令牌。 请记住，每个AEM作为Cloud Service环境都需要它自己的访问令牌。 开发的访问令牌在暂存或生产环境中不起作用，暂存环境在开发或生产环境中不起作用，而生产环境在开发或暂存环境中不起作用！

1. 使用命令行，从项目的根执行应用程序，并传递以下参数：

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Limited Use" \
       file=local_development_token.json
   ```

   传入以下参数：

   + `aem`:AEM作为应用程序将与之交互的Cloud Service环境的方案和主机名(例如， `https://author-p1234-e5678.adobeaemcloud.com`)。
   + `folder`:资产将更新为的资产文件夹路径 `propertyValue`;请勿添加 `/content/dam` 前缀(例如 `/wknd/en/adventures/napa-wine-tasting`)
   + `propertyName`:要更新的资产属性名称，相对 `[dam:Asset]/jcr:content` 于(例如 `metadata/dc:rights`)。
   + `propertyValue`:将设置为的 `propertyName` 值；包含空格的值需要使用( `"` 例如 `"WKND Limited Use"`)
   + `file`:从AEM开发人员控制台下载的JSON文件的相对文件路径。

   成功执行每个更新资产的应用程序结果输出：

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### 验证AEM中的元数据更新

通过作为Cloud Service环境登录AEM以验证元数据是否已更新（确保访问传递到`aem`命令行参数的相同主机）。

1. 以外部应用程序交互的Cloud Service环境登录AEM（使用`aem`命令行参数中提供的相同主机）
1. 导航到&#x200B;__Assets__ > __Files__
1. 导航到`folder`命令行参数指定的资产文件夹，例如&#x200B;__WKND__ > __英语__ > __冒险__ > __纳帕品酒会__
1. 打开文件夹中任何（非内容片段）资产的&#x200B;__属性__
1. 点按&#x200B;__Advanced__&#x200B;选项卡
1. 查看更新属性的值，例如&#x200B;__Copyright__，该值映射到更新的`metadata/dc:rights` JCR属性，该属性反映`propertyValue`参数中提供的值，例如&#x200B;__WKND Limited Use__

![WKND有限使用元数据更新](./assets/local-development-access-token/asset-metadata.png)

## 下面的步骤

现在，我们已使用本地开发令牌以编程方式访问AEM作为Cloud Service，因此我们需要更新应用程序以使用服务凭据进行处理，以便此应用程序可以在生产环境中使用。

+ [如何使用服务凭据](./service-credentials.md)
