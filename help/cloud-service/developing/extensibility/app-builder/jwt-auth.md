---
title: 在App Builder操作中生成访问标记
description: 了解如何使用JWT凭据生成访问令牌，以便在App Builder操作中使用。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-11743
last-substantial-update: 2023-01-17T00:00:00Z
exl-id: 9a3fed96-c99b-43d1-9dba-a4311c65e5b9
duration: 229
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 0%

---

# 在App Builder操作中生成访问标记

App Builder操作可能需要与与Adobe Developer Console项目关联的AdobeAPI进行交互，并且还将部署App Builder应用程序。

为此，可能需要执行App Builder操作以生成与所需Adobe Developer控制台项目关联的访问令牌。

>[!IMPORTANT]
>
> 审核 [App Builder安全文档](https://developer.adobe.com/app-builder/docs/guides/security/) 以了解何时应生成访问令牌而不是使用提供的访问令牌。
>
> 自定义操作可能需要提供自己的安全检查，以确保只有允许的用户才能访问App Builder操作及其后的Adobe服务。


## .env文件

在App Builder项目的 `.env` 文件，为每个Adobe Developer控制台项目的JWT凭据附加自定义密钥。 JWT凭据值可以从Adobe Developer Console项目的 __凭据__ > __服务帐户(JWT)__ 对于给定工作区。

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

以下项的值 `JWT_CLIENT_ID`， `JWT_CLIENT_SECRET`， `JWT_TECHNICAL_ACCOUNT_ID`， `JWT_IMS_ORG` 可以直接从Adobe Developer控制台项目的JWT凭据屏幕复制。

### Metascopes

确定App Builder操作与之交互的AdobeAPI及其元数据。 在中列出带有逗号分隔符的元组 `JWT_METASCOPES` 键。 中列出了有效的metascope [Adobe的JWT Metascope文档](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/Scopes/).


例如，可将以下值添加到 `JWT_METASCOPES` 键入 `.env`：

```
...
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
...
```

### 私钥

此 `JWT_PRIVATE_KEY` 必须特别设置格式，因为它本身是多行值，不支持此功能 `.env` 文件。 最简单的方法是对私钥进行base64编码。 Base64对私钥进行编码(`-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----`)可以使用操作系统提供的本机工具完成。

>[!BEGINTABS]

>[!TAB macOS]

1. 打开 `Terminal`
1. 运行命令 `base64 -i /path/to/private.key | pbcopy`
1. base64输出将自动复制到剪贴板中
1. 粘贴到 `.env` 作为相应键的值

>[!TAB Windows]

1. 打开 `Command Prompt`
1. 运行命令 `certutil -encode C:\path\to\private.key C:\path\to\encoded-private.key`
1. 运行命令 `findstr /v CERTIFICATE C:\path\to\encoded-private.key`
1. 将base64输出复制到剪贴板
1. 粘贴到 `.env` 作为相应键的值

>[!TAB Linux®]

1. 打开终端
1. 运行命令 `base64 private.key`
1. 将base64输出复制到剪贴板
1. 粘贴到 `.env` 作为相应键的值

>[!ENDTABS]

例如，可以将以下base64编码的私钥添加到 `JWT_PRIVATE_KEY` 键入 `.env`：

```
...
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

## 输入映射

在JWT凭据值设置于 `.env` 文件，必须将它们映射到AppBuilder操作输入，以便可在操作本身中读取它们。 为此，请在每个变量中添加相应的条目 `ext.config.yaml` 操作 `inputs` 采用以下格式： `PARAMS_INPUT_NAME: $ENV_KEY`.

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

下定义的键 `inputs` 可在 `params` 提供给App Builder操作的对象。


## 用于访问令牌的JWT凭据

在App Builder操作中，JWT凭据在以下位置提供： `params` 对象，可使用者 [`@adobe/jwt-auth`](https://www.npmjs.com/package/@adobe/jwt-auth) 以生成访问令牌，从而访问其他AdobeAPI和服务。

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
