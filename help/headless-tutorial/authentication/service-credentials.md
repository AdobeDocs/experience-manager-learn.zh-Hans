---
title: 服务凭据
description: AEM Service凭据用于帮助外部应用程序、系统和服务以编程方式通过HTTP与AEM创作或发布服务进行交互。
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: API
activity: develop
audience: developer
kt: 6785
thumbnail: 330519.jpg
topic: 无头、集成
role: Developer
level: Intermediate, Experienced
source-git-commit: b902ced3d7f7cf827d0a487bf741ff370f7c1f04
workflow-type: tm+mt
source-wordcount: '1863'
ht-degree: 0%

---


# 服务凭据

与AEM作为Cloud Service的集成必须能够安全地对AEM进行身份验证。 AEM开发人员控制台授予对服务凭据的访问权限，服务凭据用于促进外部应用程序、系统和服务以编程方式通过HTTP与AEM创作或发布服务交互。

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

服务凭据可能显示类似于[本地开发访问令牌](./local-development-access-token.md)，但在以下几种关键方面有所不同：

+ 服务凭据是&#x200B;_不_&#x200B;访问令牌，而是用于&#x200B;_获取_&#x200B;访问令牌的凭据。
+ 服务凭据更为永久（每365天过期一次），除非被撤销，否则不会更改，而本地开发访问令牌会每天过期。
+ AEM作为Cloud Service环境的服务凭据映射到单个AEM技术帐户用户，而本地开发访问令牌作为生成访问令牌的AEM用户进行身份验证。

应将服务凭据及其生成的访问令牌以及本地开发访问令牌均保密，因为所有这三个令牌都可用于作为Cloud Service环境访问其各自的AEM

## 生成服务凭据

服务凭据生成分为两个步骤：

1. 由AdobeIMS组织管理员进行的一次性服务凭据初始化
1. 服务凭据JSON的下载和使用

### 服务凭据初始化

与本地开发访问令牌不同，服务凭据需要由Adobe组织IMS管理员进行一次性初始化&#x200B;_，然后才能下载。_

![初始化服务凭据](assets/service-credentials/initialize-service-credentials.png)

__这是作为Cloud Service环境对每个AEM进行一次性初始化__

1. 确保您以以下方式登录：
   + 您的AdobeIMS组织的管理员
   + __Cloud Manager - Developer__ IMS产品配置文件的成员
   + __AEM用户__&#x200B;或&#x200B;__AEM管理员__ AEM作者&#x200B;__上的IMS产品配置文件的成员__
1. 登录到[AdobeCloud Manager](https://my.cloudmanager.adobe.com)
1. 打开包含AEM作为Cloud Service环境的程序，以集成
1. 点按&#x200B;__环境__&#x200B;部分中环境旁边的省略号，然后选择&#x200B;__开发人员控制台__
1. 点按&#x200B;__集成__&#x200B;选项卡中的
1. 点按&#x200B;__获取服务凭据__&#x200B;按钮
1. 服务凭据将初始化并显示为JSON

![AEM Developer Console — 集成 — 获取服务凭据](./assets/service-credentials/developer-console.png)

初始化AEM作为Cloud Service环境的服务凭据后，您的AdobeIMS组织中的其他AEM开发人员可以下载这些凭据。

### 下载服务凭据

![下载服务凭据](assets/service-credentials/download-service-credentials.png)

下载服务凭据的步骤与初始化的步骤相同。 如果尚未进行初始化，则用户点按&#x200B;__获取服务凭据__&#x200B;按钮时将收到错误。

1. 确保您已作为以下用户登录：
   + __Cloud Manager - Developer__ IMS产品配置文件的成员(用于授予对AEM Developer Console的访问权限)
      + 沙盒AEM作为Cloud Service环境不需要此&#x200B;__Cloud Manager - Developer__&#x200B;成员资格
   + __AEM用户__&#x200B;或&#x200B;__AEM管理员__ AEM作者&#x200B;__上的IMS产品配置文件的成员__
1. 登录到[AdobeCloud Manager](https://my.cloudmanager.adobe.com)
1. 打开包含AEM作为Cloud Service环境的程序以与集成
1. 点按&#x200B;__环境__&#x200B;部分中环境旁边的省略号，然后选择&#x200B;__开发人员控制台__
1. 点按&#x200B;__集成__&#x200B;选项卡中的
1. 点按&#x200B;__获取服务凭据__&#x200B;按钮
1. 点按左上角的下载按钮，下载包含服务凭据值的JSON文件，并将文件保存到安全位置。
   + _如果服务凭据受损，请立即联系Adobe支持部门以撤销它们_

## 安装服务凭据

服务凭据提供生成JWT所需的详细信息，JWT用于交换用于以AEM作为Cloud Service进行身份验证的访问令牌。 服务凭据必须存储在安全位置，外部应用程序、系统或服务可以使用服务凭据访问AEM。 每个客户管理服务凭据的方式和位置将是唯一的。

为了简单起见，本教程将通过命令行将服务凭据传递到中，但请与您的IT安全团队合作，了解如何根据贵组织的安全准则存储和访问这些凭据。

1. 将[下载的服务凭据JSON](#download-service-credentials)复制到项目根目录中名为`service_token.json`的文件中
   + 但请记住，切勿向Git提交任何凭据！

## 使用服务凭据

服务凭据（一个格式完整的JSON对象）与JWT或访问令牌不同。 服务凭据（包含私钥）而是用于生成JWT，JWT与AdobeIMS API交换以获取访问令牌。

![服务凭据 — 外部应用程序](assets/service-credentials/service-credentials-external-application.png)

1. 将服务凭据从AEM Developer Console下载到安全位置
1. 外部应用程序需要以编程方式与AEM作为Cloud Service环境进行交互
1. 外部应用程序从安全位置读取服务凭据
1. 外部应用程序使用服务凭据中的信息来构建JWT令牌
1. JWT令牌将发送到AdobeIMS以交换访问令牌
1. AdobeIMS会返回一个访问令牌，可用于将AEM作为Cloud Service访问
   + 访问令牌可能已请求到期。 最好保持访问令牌的生命周期较短，并在需要时进行刷新。
1. 外部应用程序将HTTP请求作为Cloud Service发出，从而将访问令牌作为载体令牌添加到HTTP请求的授权标头
1. AEM as a A Service接收HTTP请求、验证请求并执行HTTP请求请求所请求的工作，并将HTTP响应返回给外部应用程序

### 外部应用程序的更新

要使用服务凭据访问AEM as a Cloud Service，必须通过3种方式更新外部应用程序：

1. 在服务凭据中阅读
   + 为了简单起见，我们将从下载的JSON文件中读取这些内容，但在实际使用场景中，必须按照贵组织的安全准则安全地存储服务凭据
1. 从服务凭据生成JWT
1. 将JWT交换为访问令牌
   + 当存在服务凭据时，我们的外部应用程序在访问AEM作为Cloud Service时会使用此访问令牌而不是本地开发访问令牌

在本教程中，Adobe的`@adobe/jwt-auth` npm模块用于两者，(1)从服务凭据生成JWT，(2)在单个函数调用中将其交换为访问令牌。 如果您的应用程序不基于JavaScript，请查看其他语言的[示例代码](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)，了解如何从服务凭据创建JWT，并将其与AdobeIMS交换为访问令牌。

## 阅读服务凭据

查看`getCommandLineParams()`，并查看我们可以使用与本地开发访问令牌JSON中读取的相同代码在服务凭据JSON文件中读取。

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

读取服务凭据后，将使用它们生成JWT，然后与AdobeIMS API交换访问令牌，该令牌随后可用于作为Cloud Service访问AEM。

此示例应用程序基于Node.js，因此最好使用[@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) npm模块来促进(1)JWT生成和(20)与AdobeIMS交换。 如果您的应用程序是使用其他语言开发的，请查看[相应的代码示例](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)，了解如何使用其他编程语言构建HTTP请求以AdobeIMS。

1. 更新`getAccessToken(..)`以检查JSON文件内容，并确定它是否表示本地开发访问令牌或服务凭据。 通过检查`.accessToken`属性是否存在，可以轻松实现此目的，该属性仅存在于本地开发访问令牌JSON中。

   如果提供了服务凭据，则应用程序会生成JWT，并将其与AdobeIMS交换以获取访问令牌。 我们将使用[@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth)的`auth(...)`函数，该函数既生成JWT，又在单个函数调用中用于访问令牌。  `auth(..)`的参数是[JSON对象，由服务凭据JSON中提供的特定信息](https://www.npmjs.com/package/@adobe/jwt-auth#config-object)组成，如以下代码中所述。

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

   现在，根据通过`file`命令行参数传入的JSON文件（本地开发访问令牌JSON或服务凭据JSON），应用程序将派生访问令牌。

   请记住，尽管服务凭据每365天过期一次，但JWT和相应的访问令牌会频繁过期，并且需要在它们过期之前进行刷新。 可以使用AdobeIMS](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens)提供的`refresh_token` [来完成此操作。

1. 在进行这些更改并从AEM开发人员控制台下载服务凭据JSON（为了简便起见，另存为与此`index.js`相同的文件夹`service_token.json`）后，执行将命令行参数`file`替换为`service_token.json`的应用程序，并将`propertyValue`更新为新值，以便在AEM中显现效果。

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Restricted Use" \
       file=service_token.json
   ```

   终端的输出将如下所示：

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

   __403 - Forbidden__&#x200B;行表示在对AEM作为Cloud Service的HTTP API调用中出错。 尝试更新资产的元数据时，会出现这些403禁止错误。

   原因是服务凭据派生的访问令牌使用自动创建的技术帐户AEM用户对AEM的请求进行身份验证，默认情况下，该用户仅具有读权限。 要提供对AEM的应用程序写入访问权限，必须向与访问令牌关联的技术帐户AEM用户授予在AEM中的权限。

## 在AEM中配置访问权限

服务凭据派生的访问令牌使用的技术帐户AEM用户，该用户在参与者AEM用户组中具有成员资格。

![服务凭据 — 技术帐户AEM用户](./assets/service-credentials/technical-account-user.png)

在AEM中存在技术帐户AEM用户（在首次通过访问令牌发出HTTP请求后）后，可以像管理其他AEM用户一样管理此AEM用户的权限。

1. 首先，打开从AEM开发人员控制台下载的服务凭据JSON ，找到技术帐户的AEM登录名，然后找到`integration.email`值，该值应类似于：`12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`。
1. 以AEM管理员身份登录到相应AEM环境的创作服务
1. 导航到&#x200B;__Tools__ > __Security__ > __Users__
1. 找到在步骤1中标识了&#x200B;__登录名__&#x200B;的AEM用户，并打开其&#x200B;__属性__
1. 导航到&#x200B;__组__&#x200B;选项卡，并添加&#x200B;__DAM用户__&#x200B;组（将其作为资产的写入访问权限）
1. 点按&#x200B;__保存并关闭__

在AEM中拥有资产写入权限的技术帐户，请重新运行应用程序：

```shell
$ node index.js \
    aem=https://author-p1234-e5678.adobeaemcloud.com \
    folder=/wknd/en/adventures/napa-wine-tasting \
    propertyName=metadata/dc:rights \
    propertyValue="WKND Restricted Use" \
    file=service_token.json
```

终端的输出将如下所示：

```
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
```

## 验证更改

1. 以已更新的Cloud Service环境身份登录AEM（使用`aem`命令行参数中提供的相同主机名）
1. 导航到&#x200B;__Assets__ > __Files__
1. 导航到`folder`命令行参数指定的资产文件夹，例如&#x200B;__WKND__ > __英语__ > __冒险__ > __纳帕品酒会__
1. 打开文件夹中任何资产的&#x200B;__属性__
1. 导航到&#x200B;__Advanced__&#x200B;选项卡
1. 查看更新属性的值，例如映射到更新的`metadata/dc:rights` JCR属性的&#x200B;__Copyright__，该值现在反映`propertyValue`参数中提供的值，例如&#x200B;__WKND Restricted Use__

![WKND限制使用元数据更新](./assets/service-credentials/asset-metadata.png)

## 恭喜！

现在，我们已通过本地开发访问令牌以及生产就绪的服务到服务访问令牌，以编程方式将AEM作为Cloud Service访问！

