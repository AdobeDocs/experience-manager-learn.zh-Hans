---
title: 在App Builder操作中生成服务器到服务器访问令牌
description: 了解如何使用OAuth服务器到服务器凭据生成访问令牌以用于App Builder操作。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-14724
last-substantial-update: 2024-02-29T00:00:00Z
duration: null
source-git-commit: c77dd9c2872e7e43863d83837cedbff50a7d3c1a
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 0%

---

# 在App Builder操作中生成服务器到服务器访问令牌

App Builder操作可能需要与支持的AdobeAPI进行交互 **OAuth服务器到服务器凭据** 和与Adobe Developer Console项目关联，因此部署了App Builder应用程序。

本指南介绍如何使用生成访问令牌 _OAuth服务器到服务器凭据_ 以便在App Builder操作中使用。

>[!IMPORTANT]
>
> 服务帐户(JWT)凭据已弃用，推荐使用OAuth服务器到服务器凭据。 但是，仍然有一些AdobeAPI仅支持服务帐户(JWT)凭据，迁移到OAuth服务器到服务器的过程正在进行中。 查看AdobeAPI文档，了解支持哪些凭据。

## Adobe Developer Console项目配置

将所需的AdobeAPI添加到Adobe Developer Console项目时，请在 _配置API_ 步骤，选择 **OAuth服务器到服务器** 身份验证类型。

![Adobe Developer控制台 — OAuth服务器到服务器](./assets/s2s-auth/oauth-server-to-server.png)

要分配上述自动创建的集成服务帐户，请选择所需的产品配置文件。 因此，通过产品用户档案，服务帐户权限被控制。

![Adobe Developer控制台 — 产品配置文件](./assets/s2s-auth/select-product-profile.png)

## .env文件

在App Builder项目的 `.env` 文件，附加Adobe Developer控制台项目的OAuth服务器到服务器凭据的自定义密钥。 OAuth服务器到服务器凭据值可以从Adobe Developer控制台项目的 __凭据__ > __OAuth服务器到服务器__ 对于给定工作区。

![Adobe Developer控制台OAuth服务器到服务器凭据](./assets/s2s-auth/oauth-server-to-server-credentials.png)

```
...
OAUTHS2S_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
OAUTHS2S_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
OAUTHS2S_CECREDENTIALS_METASCOPES=AdobeID,openid,ab.manage,additional_info.projectedProductContext,read_organizations,read_profile,account_cluster.read
```

以下项的值 `OAUTHS2S_CLIENT_ID`， `OAUTHS2S_CLIENT_SECRET`， `OAUTHS2S_CECREDENTIALS_METASCOPES` 可以直接从Adobe Developer控制台项目的“OAuth服务器到服务器凭据”屏幕复制。

## 输入映射

将OAuth服务器到服务器凭据值设置为 `.env` 文件，必须将它们映射到AppBuilder操作输入，以便可在操作本身中读取它们。 为此，请在每个变量中添加相应的条目 `ext.config.yaml` 操作 `inputs` 采用以下格式： `PARAMS_INPUT_NAME: $ENV_KEY`.

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
            OAUTHS2S_CLIENT_ID: $OAUTHS2S_CLIENT_ID
            OAUTHS2S_CLIENT_SECRET: $OAUTHS2S_CLIENT_SECRET
            OAUTHS2S_CECREDENTIALS_METASCOPES: $OAUTHS2S_CECREDENTIALS_METASCOPES
          annotations:
            require-adobe-auth: false
            final: true
```

下定义的键 `inputs` 可在 `params` 提供给App Builder操作的对象。

## 用于访问令牌的OAuth服务器到服务器凭据

在App Builder操作中，OAuth服务器到服务器凭据在以下位置提供： `params` 对象。 使用这些凭据，可以使用生成访问令牌 [OAuth 2.0库](https://oauth.net/code/). 或者，您可以使用 [节点提取库](https://www.npmjs.com/package/node-fetch) 向Adobe IMS令牌端点发出POST请求以获取访问令牌。

以下示例演示如何使用 `node-fetch` 库，向Adobe IMS令牌端点发出POST请求以获取访问令牌。

```javascript
const fetch = require("node-fetch");
const { Core } = require("@adobe/aio-sdk");
const { errorResponse, stringParameters, checkMissingRequestInputs } = require("../utils");

async function main(params) {
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });

  try {
    // Perform any necessary input error checking
    const systemErrorMessage = checkMissingRequestInputs(params, ["OAUTHS2S_CLIENT_ID", "OAUTHS2S_CLIENT_SECRET", "OAUTHS2S_CECREDENTIALS_METASCOPES"], []);

    // The Adobe IMS token endpoint URL
    const adobeIMSV3TokenEndpointURL = 'https://ims-na1.adobelogin.com/ims/token/v3';

    // The POST request options
    const options = {
        method: 'POST',
        headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
        },
        body: `grant_type=client_credentials&client_id=${params.OAUTHS2S_CLIENT_ID}&client_secret=${params.OAUTHS2S_CLIENT_SECRET}&scope=${params.OAUTHS2S_CECREDENTIALS_METASCOPES}`,
    };

    // Make a POST request to the Adobe IMS token endpoint to get the access token
    const response = await fetch(adobeIMSV3TokenEndpointURL, options);
    const responseJSON = await response.json();

    // The 24-hour IMS Access Token is used to call the AEM Data Service API
    // Can look at caching this token for 24 hours to reduce calls
    const accessToken = responseJSON.access_token;

    // Invoke an AEM Data Service API using the access token
    const res = await fetch(`https://api.adobeaemcloud.com/adobe/stats/statistics/contentRequestsQuota?imsOrgId=${IMS_ORG_ID}&current=true`, {
      headers: {
        'X-Adobe-Accept-Experimental': '1',
        'x-gw-ims-org-id': IMS_ORG_ID,
        'X-Api-Key': params.OAUTHS2S_CLIENT_ID,
        Authorization: `Bearer ${access_token}`, // The 24-hour IMS Access Token
      },
      method: "GET",
    });

    if (!res.ok) { throw new Error("Request to API failed with status code " + res.status);}

    // API data
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
