---
title: Github.com webhook验证
description: 了解如何在App Builder操作中验证来自Github.com的webhook请求。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-15714
last-substantial-update: 2023-06-06T00:00:00Z
exl-id: 5492dc7b-f034-4a7f-924d-79e083349e26
source-git-commit: 8f64864658e521446a91bb4c6475361d22385dc1
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---

# Github.com webhook验证

Webhook允许您构建或设置可订阅GitHub.com上特定事件的集成。 触发其中某个事件时，GitHub会将HTTPPOST有效负载发送到webhook配置的URL。 但是，出于安全原因，请务必验证传入的webhook请求是否实际来自GitHub，而不是来自恶意操作者。 本教程将指导您完成在AdobeApp Builder操作中使用共享密钥验证GitHub.com webhook请求的步骤。

## 在AppBuilder中设置Github密钥

1. **向`.env`文件添加密钥：**

   在App Builder项目的`.env`文件中，为GitHub.com webhook密钥添加自定义密钥：

   ```env
   # Specify your secrets here
   # This file must not be committed to source control
   ...
   GITHUB_SECRET=my-github-webhook-secret-1234!
   ```

2. **更新`ext.config.yaml`文件：**

   必须更新`ext.config.yaml`文件以验证GitHub.com webhook请求。

   - 将AppBuilder操作`web`配置设置为`raw`以从GitHub.com接收原始请求正文。
   - 在AppBuilder操作配置中的`inputs`下，添加`GITHUB_SECRET`密钥，并将其映射到包含该密钥的`.env`字段。 此键的值是以`$`为前缀的`.env`字段名称。
   - 将AppBuilder操作配置中的`require-adobe-auth`注释设置为`false`以允许在不需要Adobe身份验证的情况下调用操作。

   生成的`ext.config.yaml`文件应如下所示：

   ```yaml
   operations:
     view:
       - type: web
         impl: index.html
   actions: actions
   web: web-src
   runtimeManifest:
     packages:
       dx-excshell-1:
         license: Apache-2.0
         actions:
           github-to-jira:
             function: actions/generic/index.js
             web: 'raw'
             runtime: nodejs:20
             inputs:
               LOG_LEVEL: debug
               GITHUB_SECRET: $GITHUB_SECRET
             annotations:
               require-adobe-auth: false
               final: true
   ```

## 将验证代码添加到AppBuilder操作

接下来，将下面提供的JavaScript代码（复制自[GitHub.com的文档](https://docs.github.com/en/webhooks/using-webhooks/validating-webhook-deliveries#javascript-example)）添加到您的AppBuilder操作中。 确保导出`verifySignature`函数。

```javascript
// src/dx-excshell-1/actions/generic/github-webhook-verification.js

let encoder = new TextEncoder();

async function verifySignature(secret, header, payload) {
    let parts = header.split("=");
    let sigHex = parts[1];

    let algorithm = { name: "HMAC", hash: { name: 'SHA-256' } };

    let keyBytes = encoder.encode(secret);
    let extractable = false;
    let key = await crypto.subtle.importKey(
        "raw",
        keyBytes,
        algorithm,
        extractable,
        [ "sign", "verify" ],
    );

    let sigBytes = hexToBytes(sigHex);
    let dataBytes = encoder.encode(payload);
    let equal = await crypto.subtle.verify(
        algorithm.name,
        key,
        sigBytes,
        dataBytes,
    );

    return equal;
}

function hexToBytes(hex) {
    let len = hex.length / 2;
    let bytes = new Uint8Array(len);

    let index = 0;
    for (let i = 0; i < hex.length; i += 2) {
        let c = hex.slice(i, i + 2);
        let b = parseInt(c, 16);
        bytes[index] = b;
        index += 1;
    }

    return bytes;
}

module.exports = { verifySignature };
```

## 在AppBuilder操作中实施验证

接下来，通过将请求标头中的签名与`verifySignature`函数生成的签名进行比较，验证请求是否来自GitHub。

在AppBuilder操作的`index.js`中，将以下代码添加到`main`函数中：


```javascript
// src/dx-excshell-1/actions/generic/index.js

const { verifySignature } = require("./github-webhook-verification");
...

// Main function that will be executed by Adobe I/O Runtime
async function main(params) {
  // Create a Logger
  const logger = Core.Logger("main", { level: params?.LOG_LEVEL || "info" });

  try {
    // Log parameters if LOG_LEVEL is 'debug'
    logger.debug(stringParameters(params));

    // Define required parameters and headers
    const requiredParams = [
      // Verifies the GITHUB_SECRET is present in the action's configuration; add other parameters here as needed.
      "GITHUB_SECRET"
    ];

    const requiredHeaders = [
      // Require the x-hub-signature-256 header, which GitHub.com populates with a sha256 hash of the payload
      "x-hub-signature-256"
    ];

    // Check for missing required parameters and headers
    const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders);

    if (errorMessage) {
      // Return and log client errors
      return errorResponse(400, errorMessage, logger);
    }

    // Decode the request body (which is base64 encoded) to a string
    const body = Buffer.from(params.__ow_body, 'base64').toString('utf-8');

    // Verify the GitHub webhook signature
    const isSignatureValid = await verifySignature(
      params.GITHUB_SECRET,
      params.__ow_headers["x-hub-signature-256"],
      body
    );

    if (!isSignatureValid) {
      // GitHub signature verification failed
      return errorResponse(401, "Unauthorized", logger);
    } else {
      logger.debug("Signature verified");
    }

    // Parse the request body as JSON so its data is useful in the action
    const githubParams = JSON.parse(body) || {};

    // Optionally, merge the GitHub webhook request parameters with the action parameters
    const mergedParams = {
      ...params,
      ...githubParams
    };

    // Do work based on the GitHub webhook request
    doWork(mergedParams);

    return {
      statusCode: 200,
      body: { message: "GitHub webhook received and processed!" }
    };

  } catch (error) {
    // Log any server errors
    logger.error(error);
    // Return with 500 status code
    return errorResponse(500, "Server error", logger);
  }
}
```

## 在GitHub中配置webhook

返回GitHub.com，在创建webhook时向GitHub.com提供相同的机密值。 使用在`.env`文件的`GITHUB_SECRET`密钥中指定的机密值。

在GitHub.com中，转到存储库设置，并编辑webhook。 在webhook设置中，在`Secret`字段中提供机密值。 单击底部的&#x200B;__更新webhook__&#x200B;以保存更改。

![Github Webhook密码](./assets/github-webhook-verification/github-webhook-settings.png)

按照以下步骤操作，您将确保App Builder操作可以安全地验证传入webhook请求确实来自您的GitHub.com webhook。
