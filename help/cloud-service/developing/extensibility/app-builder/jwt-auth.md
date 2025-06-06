---
title: 在App Builder操作中生成JWT访问令牌
description: 了解如何使用JWT凭据生成访问令牌以用于App Builder操作。
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-11743
last-substantial-update: 2023-01-17T00:00:00Z
exl-id: 9a3fed96-c99b-43d1-9dba-a4311c65e5b9
duration: 151
source-git-commit: bb4f9982263a15f18b9f39b1577b61310dfbe643
workflow-type: tm+mt
source-wordcount: '456'
ht-degree: 1%

---

# 在App Builder操作中生成JWT访问令牌

App Builder操作可能需要与与Adobe项目关联的Adobe Developer Console API交互，App Builder应用程序也将需要部署。

这可能要求App Builder操作生成与所需Adobe Developer Console项目关联的自己的JWT访问令牌。

>[!IMPORTANT]
>
> 查看[App Builder安全文档](https://developer.adobe.com/app-builder/docs/guides/security/)以了解何时生成访问令牌与使用提供的访问令牌相比较合适。
>
> 自定义操作可能需要提供自己的安全检查，以确保仅允许的使用者可以访问App Builder操作及其后面的Adobe服务。


## .env文件

在App Builder项目的`.env`文件中，为每个Adobe Developer Console项目的JWT凭据附加自定义密钥。 可以从给定工作区的Adobe Developer Console项目的&#x200B;__凭据__ > __服务帐户(JWT)__&#x200B;获取JWT凭据值。

![Adobe Developer Console JWT服务凭据](./assets/jwt-auth/jwt-credentials.png)

```
...
JWT_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
JWT_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
JWT_TECHNICAL_ACCOUNT_ID=1F072B8A63C6E0230A495EE1@techacct.adobe.com
JWT_IMS_ORG=7ABB3E6A5A7491460A495D61@AdobeOrg
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

可以从Adobe Developer Console项目的JWT凭据屏幕直接复制`JWT_CLIENT_ID`、`JWT_CLIENT_SECRET`、`JWT_TECHNICAL_ACCOUNT_ID`、`JWT_IMS_ORG`的值。

### Metascopes

确定App Builder操作与之交互的Adobe API及其元数据。 在`JWT_METASCOPES`键中列出带有逗号分隔符的metascope。 [Adobe的JWT Metascope文档](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/scopes)中列出了有效的Metascope。


例如，可以将以下值添加到`.env`中的`JWT_METASCOPES`键中：

```
...
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
...
```

### 私钥

`JWT_PRIVATE_KEY`必须经过特殊格式设置，因为它本身是多行值，在`.env`文件中不支持该值。 最简单的方法是对私钥进行base64编码。 可以使用操作系统提供的本机工具对私钥(`-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----`)进行Base64编码。

>[!BEGINTABS]

>[!TAB macOS]

1. 打开`Terminal`
1. 运行命令`base64 -i /path/to/private.key | pbcopy`
1. base64输出将自动复制到剪贴板中
1. 将值粘贴到`.env`中以作为相应键的值

>[!TAB Windows]

1. 打开`Command Prompt`
1. 运行命令`certutil -encode C:\path\to\private.key C:\path\to\encoded-private.key`
1. 运行命令`findstr /v CERTIFICATE C:\path\to\encoded-private.key`
1. 将base64输出复制到剪贴板
1. 将值粘贴到`.env`中以作为相应键的值

>[!TAB Linux®]

1. 打开终端
1. 运行命令`base64 private.key`
1. 将base64输出复制到剪贴板
1. 将值粘贴到`.env`中以作为相应键的值

>[!ENDTABS]

例如，可以将以下base64编码私钥添加到`.env`中的`JWT_PRIVATE_KEY`密钥：

```
...
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

## 输入映射

在`.env`文件中设置JWT凭据值后，必须将它们映射到AppBuilder操作输入，以便在操作本身中读取它们。 为此，请在`ext.config.yaml`操作`inputs`中为每个变量添加条目，格式为： `PARAMS_INPUT_NAME: $ENV_KEY`。

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

在`inputs`下定义的键在提供给App Builder操作的`params`对象上可用。


## 用于访问令牌的JWT凭据

在App Builder操作中，JWT凭据在`params`对象中可用，可由[`@adobe/jwt-auth`](https://www.npmjs.com/package/@adobe/jwt-auth)用于生成访问令牌，该令牌进而可以访问其他Adobe API和服务。

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
