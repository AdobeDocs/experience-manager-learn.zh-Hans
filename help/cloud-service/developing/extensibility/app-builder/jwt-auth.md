---
title: 在应用程序生成器操作中生成访问令牌
description: 了解如何使用JWT凭据生成访问令牌，以在应用程序生成器操作中使用。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
kt: 11743
last-substantial-update: 2023-01-17T00:00:00Z
source-git-commit: 0990fc230e2a36841380b5b0c6cd94dca24614fa
workflow-type: tm+mt
source-wordcount: '434'
ht-degree: 1%

---


# 在应用程序生成器操作中生成访问令牌

应用程序生成器操作可能需要与与与部署了应用程序生成器应用程序的Adobe Developer控制台项目关联的AdobeAPI进行交互。

这可能需要应用程序生成器操作才能生成与所需的Adobe Developer控制台项目关联的其自身访问令牌。

>[!IMPORTANT]
>
> 审阅 [App Builder安全文档](https://developer.adobe.com/app-builder/docs/guides/security/) 了解与使用提供的访问令牌相比，何时生成访问令牌更合适。
>
> 自定义操作可能需要自行提供安全检查，以确保只有允许的消费者才能访问应用程序生成器操作及其后的Adobe服务。


## .env文件

在应用程序生成器项目的 `.env` 文件中，为每个Adobe Developer控制台项目的JWT凭据附加自定义键。 JWT凭据值可以从Adobe Developer控制台项目的 __凭据__ > __服务帐户(JWT)__ （对于给定工作区）。

![Adobe Developer控制台JWT服务凭据](./assets/jwt-auth/jwt-credentials.png)

```
...
JWT_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
JWT_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
JWT_TECHNICAL_ACCOUNT_ID=1F072B8A63C6E0230A495EE1@techacct.adobe.com
JWT_IMS_ORG=7ABB3E6A5A7491460A495D61@AdobeOrg
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

的值 `JWT_CLIENT_ID`, `JWT_CLIENT_SECRET`, `JWT_TECHNICAL_ACCOUNT_ID`, `JWT_IMS_ORG` 可以直接从Adobe Developer控制台项目的JWT凭据屏幕中复制。

### 元塔科佩

确定AdobeAPI及其元数据，以便应用程序生成器操作与之交互。 在 `JWT_METASCOPES` 键。 有效的元数据列在 [Adobe的JWT Metascope文档](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/Scopes/).


例如，可以将以下值添加到 `JWT_METASCOPES` 键 `.env`:

```
...
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
...
```

### 私钥

的 `JWT_PRIVATE_KEY` 必须特别设置格式，因为它本身是多行值，在 `.env` 文件。 最简单的方法是对私钥进行Base64编码。 Base64编码私钥(`-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----`)可以使用操作系统提供的本机工具来完成。

>[!BEGINTABS]

>[!TAB macOS]

1. 打开 `Terminal`
1. `$ base64 -i /path/to/private.key | pbcopy`

base64输出将自动复制剪贴板

>[!TAB Windows]

1. 打开 `Command Prompt`
1. `$ certutil -encode C:\path\to\private.key C:\path\to\encoded-private.key`
1. 复制的内容 `encoded-private.key` 到剪贴板

>[!TAB Linux®]

1. 开放式终端
1. `$ base64 private.key`
1. 将base64输出复制到剪贴板

>[!ENDTABS]

例如，可以将以下值添加到 `JWT_PRIVATE_KEY` 键 `.env`:

```
...
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

## 扩展配置

在 `.env` 文件，则必须将它们映射到AppBuilder操作输入，以便在操作本身中读取它们。 为此，请在 `ext.config.yaml` 操作 `inputs` 格式： `INPUT_NAME=$ENV_KEY`.

例如：

```yaml
operations:
  view:
    - type: web
      impl: index.html
actions: actions
runtimeManifest:
  packages:
    dx-excshell-1:
      license: Apache-2.0
      actions:
        generic:
          function: actions/generic/index.js
          web: 'yes'
          runtime: nodejs:16
          inputs:
            LOG_LEVEL: debug
            JWT_CLIENT_ID: $JWT_CLIENT_ID
            JWT_TECHNICAL_ACCOUNT_ID: $JWT_TECHNICAL_ACCOUNT_ID
            JWT_IMS_ORG: $JWT_IMS_ORG
            JWT_METASCOPES: $JWT_METASCOPES
            JWT_PRIVATE_KEY: $JWT_PRIVATE_KEY
          annotations:
            require-adobe-auth: false
            final: true
```

下定义的键 `inputs` 在 `params` 对象。


## 将JWT凭据转换为访问令牌

在应用程序生成器操作中，JWT凭据在 `params` 对象，可用方式为 [`@adobe/jwt-auth`](https://www.npmjs.com/package/@adobe/jwt-auth) ，以生成访问令牌，而访问令牌又可以访问其他AdobeAPI和服务。

```javascript
const fetch = require("node-fetch");
const { Core } = require("@adobe/aio-sdk");
const { errorResponse, stringParameters, checkMissingRequestInputs } = require("../utils");
const auth = require("@adobe/jwt-auth");

async function main(params) {
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });

  try {
    // Perform any necessary input error checking
    const systemErrorMessage = checkMissingRequestInputs(params, [
            "JWT_CLIENT_ID", "JWT_TECHNICAL_ACCOUNT_ID", "JWT_IMS_ORG", "JWT_CLIENT_SECRET", "JWT_METASCOPES", "JWT_PRIVATE_KEY"], []);

    // Split the metascopes into an array (they are comma delimited in the .env file)
    const metascopes = params.JWT_METASCOPES?.split(',') || [];

    // Base64 decode the private key value
    const privateKey = Buffer.from(params.JWT_PRIVATE_KEY, 'base64').toString('utf-8');

    // Exchange the JWT credentials for an 24-hour Access Token
    let { accessToken } = await auth({
      clientId: params.JWT_CLIENT_ID,                          // Client Id
      technicalAccountId: params.JWT_TECHNICAL_ACCOUNT_ID,     // Technical Account Id
      orgId: params.JWT_IMS_ORG,                               // Adobe IMS Org Id
      clientSecret: params.JWT_CLIENT_SECRET,                  // Client Secret
      metaScopes: metascopes,                                  // Metadcopes defining level of access the access token should provide
      privateKey: privateKey,                                  // Private Key to sign the JWT
    });

    // The 24-hour IMS Access Token is used to call the Analytics APIs
    // Can look at caching this token for 24 hours to reduce calls
    const accessToken = await getAccessToken(params);

    // Invoke an exmaple Adobe API endpoint using the generated accessToken
    const res = await fetch('https://analytics.adobe.io/api/example/reports', {
      headers: {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "X-Proxy-Global-Company-Id": 'example',
        "Authorization": `Bearer ${accessToken}`,
        "x-Api-Key": params.JWT_CLIENT_ID,
      },
      method: "POST",
      body: JSON.stringify({... An Analytics query ... }),
    });

    if (!res.ok) { throw new Error("Request to API failed with status code " + res.status);}

    // Analytics API data
    let data = await res.json();

    const response = {
      statusCode: 200,
      body: data,
    };

    return response;
  } catch (error) {
    logger.error(error);
    return errorResponse(500, "server error", logger);
  }
}

exports.main = main;
```
