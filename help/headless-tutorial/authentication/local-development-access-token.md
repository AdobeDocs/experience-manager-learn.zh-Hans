---
title: 本地开发访问令牌
description: AEM本地开发访问令牌用于加速与AEM集成的开发，作为一个通过HTTP与AEM作者或发布服务进行有序交互的Cloud Service。
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330477.jpg
translation-type: tm+mt
source-git-commit: eabd8650886fa78d9d177f3c588374a443ac1ad6
workflow-type: tm+mt
source-wordcount: '1044'
ht-degree: 0%

---


# 本地开发访问令牌

构建需要以编程方式访问AEM作为Cloud Service的集成的开发者需要一种简单、快速的方法来获取AEM的临时访问令牌，以便于本地开发活动。 为满足这一需求，AEM Developer Console允许开发人员自行生成临时访问令牌，这些临时可用于以编程方式访问AEM。

>[!VIDEO](https://video.tv.adobe.com/v/330477/?quality=12&learn=on)

## 生成本地开发访问令牌

![获得本地开发访问令牌](assets/local-development-access-token/getting-a-local-development-access-token.png)

本地开发访问令牌提供对AEM作者和发布服务的访问权限（以生成令牌的用户身份）及其权限。 尽管这是开发令牌，但请勿共享此令牌。

1. 在[AdobeAdminConsole](https://adminconsole.adobe.com/)中，确保您（开发人员）是以下成员：
   + __云管理器——开__ 发人员IMS产品用户档案(授予访问AEM开发人员控制台的权限)
   + __AEM管理员__&#x200B;或&#x200B;__AEM用户__ IMS产品用户档案，用于AEM环境的服务，访问令牌将与
   + 沙箱AEM作为Cloud Service环境，只需&#x200B;__AEM Administrators__&#x200B;或&#x200B;__AEM Users__&#x200B;产品用户档案中的成员资格
1. 登录[Adobe云管理器](https://my.cloudmanager.adobe.com)
1. 打开包含AEM的项目作为Cloud Service环境以与
1. 点按&#x200B;__环境__&#x200B;部分中环境旁的&#x200B;__省略号__，然后选择&#x200B;__开发人员控制台__
1. 点按&#x200B;__集成__&#x200B;选项卡
1. 点按&#x200B;__获取本地开发令牌__&#x200B;按钮
1. 点按左上角的&#x200B;__下载按钮__，下载包含`accessToken`值的JSON文件，并将JSON文件保存到开发机器上的安全位置。
   + 这是您24小时开发人员访问令牌AEM的Cloud Service环境。

![AEM开发人员控制台——集成——获取本地开发令牌](./assets/local-development-access-token/developer-console.png)

## 下载本地开发访问令牌{#download-local-development-access-token}

![本地开发访问令牌-外部应用程序](assets/local-development-access-token/local-development-access-token-external-application.png)

1. 从AEM开发人员控制台下载临时本地开发访问令牌
   + 本地开发访问令牌每24小时过期一次，因此开发人员每天需要下载新的访问令牌
1. 正在开发一个以编程方式与AEM交互作为Cloud Service的外部应用程序
1. 本地开发访问令牌中的外部应用程序读取
1. 外部应用程序将发往AEM的HTTP请求构建为Cloud Service，将本地开发访问令牌添加为HTTP请求的授权头的承载令牌
1. AEM作为Cloud Service接收HTTP请求、验证请求并执行HTTP请求所请求的工作，并将HTTP响应返回给外部应用程序

### 外部应用程序

我们将创建一个简单的外部JavaScript应用程序，以说明如何使用本地开发者访问令牌通过HTTPS以Cloud Service方式有计划地访问AEM。 这说明了在AEM之外运行的&#x200B;_任何_&#x200B;应用程序或系统（无论框架或语言如何）如何使用访问令牌以编程方式验证AEM并将其作为Cloud Service访问。 在[下一节](./service-credentials.md)中，我们将更新此应用程序代码，以支持生成用于生产用途的令牌的方法。

此应用程序从命令行运行，并使用AEM AssetsHTTP API更新AEM资产元数据，流程如下：

1. 从命令行读取参数(`getCommandLineParams()`)
1. 获取用于验证AEM的访问令牌作为Cloud Service(`getAccessToken(...)`)
1. 列表在命令行参数(`listAssetsByFolder(...)`)中指定的AEM资产文件夹中的所有资产
1. 使用命令行参数(`updateMetadata(...)`)中指定的值更新所列资产的元数据

使用访问令牌以编程方式对AEM进行身份验证的关键元素是将授权HTTP请求头添加到向AEM发出的所有HTTP请求中，格式如下：

+ `Authorization: Bearer ACCESS_TOKEN`

## 运行外部应用程序

1. 确保本地开发机器上已安装[Node.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js)，该机器将用于运行外部应用程序
1. 下载并解压缩[示例外部应用程序](./assets/aem-guides_token-authentication-external-application.zip)
1. 在此项目文件夹的命令行中运行`npm install`
1. 将下载的[本地开发访问令牌](#download-local-development-access-token)复制到项目根目录中名为`local_development_token.json`的文件
   + 但请记住，千万不要向Git提交任何凭据！
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

   查看`listAssetsByFolder(...)`和`updateMetadata(...)`中的`fetch(..)`调用，注意`headers`定义值为`Bearer <ACCESS TOKEN>`的`Authorization` HTTP请求标头。 这是从外部应用程序发出的HTTP请求作为Cloud Service验证到AEM的方式。

   ```javascript
   ...
   return fetch(`${params.aem}${ASSETS_HTTP_API}${folder}.json?configid=ims`, {
               method: 'get',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
   })...
   ```

   对AEM的任何HTTP请求都必须在“授权”标头中设置“承载”访问令牌。 请记住，每个AEM都是Cloud Service环境，它需要自己的访问令牌。 开发的访问令牌不在阶段或生产上工作，阶段的不在开发或生产上工作，生产的不在开发或阶段工作！

1. 使用命令行，从项目的根执行应用程序，传递以下参数：

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Limited Use" \
       file=local_development_token.json
   ```

   传入以下参数：

   + `aem`:作为应用程序将与之交互的Cloud Service环境的AEM的方案和主机名(例如， `https://author-p1234-e5678.adobeaemcloud.com`)。
   + `folder`:资产文件夹路径，其资产将使用进行更新 `propertyValue`;请勿添加前 `/content/dam` 缀(例如 `/wknd/en/adventures/napa-wine-tasting`)
   + `propertyName`:要更新的资产属性名称，相对 `[dam:Asset]/jcr:content` 于(例如 `metadata/dc:rights`)。
   + `propertyValue`:要设置为的 `propertyName` 值；包含空格的值需要用( `"` 例如)封装 `"WKND Limited Use"`)
   + `file`:从AEM Developer Console下载的JSON文件的相对文件路径。

   成功执行每个已更新资产的应用程序结果输出：

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### 验证AEM中的元数据更新

通过以Cloud Service环境身份登录到AEM来验证元数据是否已更新（确保访问传入`aem`命令行参数的同一主机）。

1. 作为外部应用程序与之交互的Cloud Service环境登录AEM（使用`aem`命令行参数中提供的同一主机）
1. 导航到&#x200B;__资产__ > __文件__
1. 导航到由`folder`命令行参数指定的资产文件夹，例如&#x200B;__WKND__ > __英语__ > __冒险__ > __纳帕品酒会__
1. 打开文件夹中任意（非内容片段）资产的&#x200B;__属性__
1. 点按&#x200B;__高级__&#x200B;选项卡
1. 查看已更新属性的值，例如映射到已更新的`metadata/dc:rights` JCR属性的&#x200B;__Copyright__，该属性反映在`propertyValue`参数中提供的值，例如&#x200B;__WKND Limited Use__

![WKND有限使用元数据更新](./assets/local-development-access-token/asset-metadata.png)

## 后续步骤

既然我们已经使用本地开发令牌以编程方式将AEM作为Cloud Service访问，我们需要将代码更新到。