---
title: 服务凭据
description: 了解如何使用用于促进外部应用程序、系统和服务以编程方式通过HTTP与创作或发布服务交互的服务凭据。
version: Cloud Service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330519.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2023-01-12T00:00:00Z
exl-id: e2922278-4d0b-4f28-a999-90551ed65fb4
source-git-commit: 8b6d8d99c806e782a1ddce2b300211f8d4c9da56
workflow-type: tm+mt
source-wordcount: '1931'
ht-degree: 0%

---

# 服务凭据

与Adobe Experience Manager(AEM)as a Cloud Service的集成必须能够安全地验证AEM服务。 AEM开发人员控制台授予对服务凭据的访问权限，服务凭据用于促进外部应用程序、系统和服务以编程方式通过HTTP与AEM创作或发布服务交互。

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

服务凭据可能显示类似 [本地开发访问令牌](./local-development-access-token.md) 但在几个关键方面却有所不同：

+ 服务凭据与技术帐户关联。 一个技术帐户可以使用多个服务凭据。
+ 服务凭据为 _not_ 访问令牌，而不是用于 _获取_ 访问令牌。
+ 服务凭据更为永久（其证书每365天过期），除非被撤消，否则不会更改，而本地开发访问令牌会每天过期。
+ AEMas a Cloud Service环境的服务凭据映射到单个AEM技术帐户用户，而本地开发访问令牌将验证为生成访问令牌的AEM用户。
+ 一个AEMas a Cloud Service环境最多可以拥有10个技术帐户，每个帐户具有自己的服务凭据，每个帐户都映射到离散的技术帐户AEM用户。

应将服务凭据及其生成的访问令牌和本地开发访问令牌均保密。 由于所有这三种环境均可用于获取，因此可以访问各自的AEMas a Cloud Service环境。

## 生成服务凭据

服务凭据生成分为两个步骤：

1. 由Adobe IMS组织管理员一次性创建技术帐户
1. 技术帐户的服务凭据JSON的下载和使用

### 创建技术帐户

与本地开发访问令牌不同，服务凭据要求Adobe组织IMS管理员先创建技术帐户，然后才能下载。 应为每个需要以编程方式访问AEM的客户端创建离散技术帐户。

![创建技术帐户](assets/service-credentials/initialize-service-credentials.png)

技术帐户只创建一次，但随着时间的推移，可以管理用于管理与技术帐户关联的服务凭据的私钥。 例如，必须在当前私钥过期之前生成新的私钥/服务凭据，以便用户能够不间断地访问服务凭据。

1. 确保您已作为以下用户登录：
   + __Adobe IMS组织的管理员__
   + 成员 __AEM管理员__ 上的IMS产品配置文件 __AEM作者__
1. 登录到 [AdobeCloud Manager](https://my.cloudmanager.adobe.com)
1. 打开包含AEMas a Cloud Service环境的程序，以集成为
1. 点按中环境旁边的省略号 __环境__ ，然后选择 __开发人员控制台__
1. 在中点按 __集成__ 选项卡
1. 点按 __技术帐户__ 选项卡
1. 点按 __创建新技术帐户__ 按钮
1. 技术帐户的服务凭据已初始化，并显示为JSON

![AEM Developer Console — 集成 — 获取服务凭据](./assets/service-credentials/developer-console.png)

初始化AEM作为Cloud Service环境的服务凭据后，您的Adobe IMS组织中的其他AEM开发人员可以下载这些凭据。

### 下载服务凭据

![下载服务凭据](assets/service-credentials/download-service-credentials.png)

下载服务凭据的步骤与初始化步骤类似。

1. 确保您已作为以下用户登录：
   + __Adobe IMS组织的管理员__
   + 成员 __AEM管理员__ 上的IMS产品配置文件 __AEM作者__
1. 登录到 [AdobeCloud Manager](https://my.cloudmanager.adobe.com)
1. 打开包含AEMas a Cloud Service环境的程序以与集成
1. 点按中环境旁边的省略号 __环境__ ，然后选择 __开发人员控制台__
1. 在中点按 __集成__ 选项卡
1. 点按 __技术帐户__ 选项卡
1. 展开 __技术帐户__ 使用
1. 展开 __私钥__ 将下载其服务凭据，并验证状态是否为 __活动__
1. 点按 __...__ > __查看__ 与 __私钥__，显示服务凭据JSON
1. 点按左上角的下载按钮，下载包含服务凭据值的JSON文件，并将文件保存到安全位置

## 安装服务凭据

服务凭据提供生成JWT所需的详细信息，JWT用于交换用于通过AEMas a Cloud Service进行身份验证的访问令牌。 服务凭据必须存储在安全位置，外部应用程序、系统或服务可使用服务凭据访问AEM。 每个客户管理服务凭据的方式和位置都是唯一的。

为了简单起见，本教程将通过命令行传递服务凭据。 但是，请与您的IT安全团队合作，了解如何根据贵组织的安全准则存储和访问这些凭据。

1. 复制 [下载了服务凭据JSON](#download-service-credentials) 到名为 `service_token.json` 在项目的根中
   + 记住，永远不要承诺 _任何凭据_ 去Git!

## 使用服务凭据

服务凭据（一个格式完整的JSON对象）与JWT或访问令牌不同。 服务凭据（包含私钥）而是用于生成JWT，JWT将与Adobe IMS API交换以获取访问令牌。

![服务凭据 — 外部应用程序](assets/service-credentials/service-credentials-external-application.png)

1. 将服务凭据从AEM Developer Console下载到安全位置
1. 外部应用程序需要以编程方式与AEMas a Cloud Service环境交互
1. 外部应用程序从安全位置读取服务凭据
1. 外部应用程序使用服务凭据中的信息来构建JWT令牌
1. JWT令牌将发送到Adobe IMS以交换访问令牌
1. Adobe IMS会返回一个访问令牌，可用于访问AEMas a Cloud Service
   + 访问令牌可以请求过期。 最好保持访问令牌的生命周期较短，并在需要时进行刷新。
1. 外部应用程序向AEM发出HTTP请求，并将访问令牌作为载体令牌添加到HTTP请求的授权标头中
1. AEMas a Cloud Service接收HTTP请求、验证请求并执行HTTP请求请求所请求的工作，并将HTTP响应返回到外部应用程序

### 外部应用程序的更新

要使用服务凭据访问AEMas a Cloud Service，必须通过三种方式更新外部应用程序：

1. 在服务凭据中阅读

+ 为简便起见，服务凭据是从下载的JSON文件中读取的，但在实际使用场景中，必须按照贵组织的安全准则安全地存储服务凭据

1. 从服务凭据生成JWT
1. 将JWT交换为访问令牌

+ 当存在服务凭据时，外部应用程序在访问AEMas a Cloud Service时会使用此访问令牌而不是本地开发访问令牌

在本教程中，Adobe `@adobe/jwt-auth` npm模块用于两者，(1)从服务凭据生成JWT，(2)在单个函数调用中将其交换为访问令牌。 如果您的应用程序不基于JavaScript，请查看 [其他语言的示例代码](https://developer.adobe.com/developer-console/docs/guides/) 以了解如何从服务凭据创建JWT，并将其与Adobe IMS交换为访问令牌。

## 阅读服务凭据

查看 `getCommandLineParams()` 因此，请参阅如何使用本地开发访问令牌JSON中用于读取的相同代码读取服务凭据JSON文件。

```javascript
function getCommandLineParams() {
    ...

    // Read in the credentials from the provided JSON file
    // Since both the Local Development Access Token and Service Credentials files are JSON, this same approach can be re-used
    if (parameters.file) {
        parameters.developerConsoleCredentials = JSON.parse(fs.readFileSync(parameters.file));
    }

    ...
    return parameters;
}
```

## 为访问令牌创建JWT和交换

读取服务凭据后，将使用它们生成JWT，然后与Adobe IMS API交换JWT以获取访问令牌。 然后，可以使用此访问令牌访问AEMas a Cloud Service。

此示例应用程序基于Node.js，因此最好使用 [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) npm模块，以便于(1)生成JWT并（20次与Adobe IMS交换） 如果您的应用程序是使用其他语言开发的，请查看 [相应的代码示例](https://developer.adobe.com/developer-console/docs/guides/) 了解如何使用其他编程语言构建到Adobe IMS的HTTP请求。

1. 更新 `getAccessToken(..)` 用于检查JSON文件内容并确定它是否表示本地开发访问令牌或服务凭据。 这可以通过检查 `.accessToken` 属性，该属性仅存在于本地开发访问令牌JSON中。

   如果提供了服务凭据，则应用程序会生成JWT并与Adobe IMS交换JWT以获取访问令牌。 使用 [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth)&#39;s `auth(...)` 函数，该函数生成JWT并在单个函数调用中为访问令牌交换JWT。 的参数 `auth(..)` 方法是 [由特定信息组成的JSON对象](https://www.npmjs.com/package/@adobe/jwt-auth#config-object) 可从服务凭据JSON中获取，如代码中所述。

```javascript
 async function getAccessToken(developerConsoleCredentials) {

     if (developerConsoleCredentials.accessToken) {
         // This is a Local Development access token
         return developerConsoleCredentials.accessToken;
     } else {
         // This is the Service Credentials JSON object that must be exchanged with Adobe IMS for an access token
         let serviceCredentials = developerConsoleCredentials.integration;

         // Use the @adobe/jwt-auth library to pass the service credentials generated a JWT and exchange that with Adobe IMS for an access token.
         // If other programming languages are used, please see these code samples: https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md
         let { access_token } = await auth({
             clientId: serviceCredentials.technicalAccount.clientId, // Client Id
             technicalAccountId: serviceCredentials.id,              // Technical Account Id
             orgId: serviceCredentials.org,                          // Adobe IMS Org Id
             clientSecret: serviceCredentials.technicalAccount.clientSecret, // Client Secret
             privateKey: serviceCredentials.privateKey,              // Private Key to sign the JWT
             metaScopes: serviceCredentials.metascopes.split(','),   // Meta Scopes defining level of access the access token should provide
             ims: `https://${serviceCredentials.imsEndpoint}`,       // IMS endpoint used to obtain the access token from
         });

         return access_token;
     }
 }
```

    现在，根据通过“file”命令行参数传入的JSON文件（本地开发访问令牌JSON或服务凭据JSON），应用程序将派生访问令牌。
    
    请记住，尽管服务凭据每365天过期一次，但JWT和相应的访问令牌会频繁过期，并且需要在它们过期之前进行刷新。 可以使用“refresh_token”[由Adobe IMS提供](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens)来完成此操作。

1. 实施这些更改后，服务凭据JSON从AEM开发人员控制台中下载，并且为了简单起见，另存为 `service_token.json` 在与此相同的文件夹中 `index.js`. 现在，让我们执行替换命令行参数的应用程序 `file` with `service_token.json`，并更新 `propertyValue` 值，以便在AEM中显示效果。

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd-shared/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Restricted Use" \
       file=service_token.json
   ```

   到终端的输出如下所示：

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

   的 __403 — 禁止__ 行，指示对AEMas a Cloud Service的HTTP API调用中出错。 尝试更新资产的元数据时，会出现这些403禁止错误。

   原因是服务凭据派生的访问令牌使用自动创建的技术帐户AEM用户对AEM的请求进行身份验证，默认情况下，该用户仅具有读取访问权限。 要提供对AEM的应用程序写入访问权限，必须向与访问令牌关联的技术帐户AEM用户授予在AEM中的权限。

## 在AEM中配置访问权限

服务凭据派生的访问令牌使用的技术帐户AEM用户在 __参与者__ AEM用户组。

![服务凭据 — 技术帐户AEM用户](./assets/service-credentials/technical-account-user.png)

在AEM中存在技术帐户AEM用户（在首次通过访问令牌发出HTTP请求后）后，可以像管理其他AEM用户一样管理此AEM用户的权限。

1. 首先，打开从AEM开发人员控制台下载的服务凭据JSON ，找到技术帐户的AEM登录名，然后找到 `integration.email` 值，它应类似于： `12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`.
1. 以AEM管理员身份登录到相应AEM环境的创作服务
1. 导航到 __工具__ > __安全性__ > __用户__
1. 在AEM用户的 __登录名__ 在步骤1中标识，并打开 __属性__
1. 导航到 __群组__ ，然后添加 __DAM用户__ 群组（用作对资产的写入访问权限）
1. 点按 __保存并关闭__

在AEM中允许具有资产写入权限的技术帐户下，重新运行应用程序：

```shell
$ node index.js \
    aem=https://author-p1234-e5678.adobeaemcloud.com \
    folder=/wknd-shared/en/adventures/napa-wine-tasting \
    propertyName=metadata/dc:rights \
    propertyValue="WKND Restricted Use" \
    file=service_token.json
```

到终端的输出如下所示：

```
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
```

## 验证更改

1. 登录已更新的AEMas a Cloud Service环境(使用 `aem` 命令行参数)
1. 导航到 __资产__ > __文件__
1. 在指定的资产文件夹中导航它 `folder` 命令行参数，例如 __WKND__ > __英语__ > __冒险__ > __纳帕品酒会__
1. 打开 __属性__ 文件夹中的任何资产
1. 导航到 __高级__ 选项卡
1. 例如，查看已更新属性的值 __版权__ 已映射到已更新的 `metadata/dc:rights` JCR属性，该属性现在反映 `propertyValue` 参数，例如 __WKND受限使用__

![WKND限制使用元数据更新](./assets/service-credentials/asset-metadata.png)

## 恭喜！

现在，我们已使用本地开发访问令牌和生产就绪的服务到服务访问令牌以编程方式访问AEMas a Cloud Service!
