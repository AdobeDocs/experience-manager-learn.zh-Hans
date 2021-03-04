---
title: 服务凭据
description: AEM服务凭据用于帮助外部应用程序、系统和服务通过HTTP与AEM作者或发布服务进行有序交互。
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: API
activity: develop
audience: developer
kt: 6785
thumbnail: 330519.jpg
topic: “无外设、集成”
role: 开发人员
level: “中级，有经验”
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '1830'
ht-degree: 0%

---


# 服务凭据

与AEM作为Cloud Service集成必须能够安全地验证到AEM。 AEM开发人员控制台授予对服务凭据的访问权限，服务凭据用于帮助外部应用程序、系统和服务通过HTTP与AEM作者或发布服务进行有序交互。

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

服务凭据可能显示类似的[本地开发访问令牌](./local-development-access-token.md)，但在以下几种主要方面有所不同：

+ 服务凭据是&#x200B;_不是_&#x200B;访问令牌，而是用于&#x200B;_获取_&#x200B;访问令牌的凭据。
+ 服务凭据更为永久（每365天过期），除非撤销，否则不会更改，而本地开发访问令牌则每天过期。
+ AEM作为Cloud Service环境的服务凭据映射到单个AEM技术帐户用户，而本地开发访问令牌将验证为生成访问令牌的AEM用户。

服务凭据及其生成的访问令牌以及本地开发访问令牌都应保密，因为这三个应用程序都可以用作访问各自AEM的Cloud Service环境

## 生成服务凭据

服务凭据生成分为两个步骤：

1. 由AdobeIMS组织管理员进行的一次性服务凭据初始化
1. 下载和使用服务凭据JSON

### 服务凭据初始化

与本地开发访问令牌不同，服务凭据需要Adobe组织IMS管理员执行一次性初始化&#x200B;__，然后才能下载。

![初始化服务凭据](assets/service-credentials/initialize-service-credentials.png)

__这是作为Cloud Service环境，每个AEM一次性初始化__

1. 确保您以AdobeIMS组织管理员身份登录
1. 登录到[Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. 打开包含AEM的项目作为Cloud Service环境，以集成
1. 点按&#x200B;__环境__&#x200B;部分中环境旁的省略号，然后选择&#x200B;__开发人员控制台__
1. 点按&#x200B;__集成__&#x200B;选项卡
1. 点按&#x200B;__获取服务凭据__&#x200B;按钮
1. 服务凭据将初始化并显示为JSON

![AEM开发人员控制台 — 集成 — 获取服务凭据](./assets/service-credentials/developer-console.png)

一旦AEM作为Cloud Service环境的服务凭据已初始化，您的Adobe IMS组织中的其他AEM开发人员就可以下载这些凭据。

### 下载服务凭据

![下载服务凭据](assets/service-credentials/download-service-credentials.png)

下载服务凭据的步骤与初始化的步骤相同。 如果尚未进行初始化，则用户将收到一个错误，点击&#x200B;__获取服务凭据__&#x200B;按钮。

1. 确保您是&#x200B;__云管理器 — 开发人员__ IMS产品用户档案(授予对AEM开发人员控制台的访问权限)的成员
   + 沙箱AEM作为Cloud Service环境，仅要求在&#x200B;__AEM Administrators__&#x200B;或&#x200B;__AEM Users__&#x200B;产品用户档案中具有成员资格
1. 登录到[Adobe Cloud Manager](https://my.cloudmanager.adobe.com)
1. 打开包含AEM的项目作为Cloud Service环境，以与
1. 点按&#x200B;__环境__&#x200B;部分中环境旁的省略号，然后选择&#x200B;__开发人员控制台__
1. 点按&#x200B;__集成__&#x200B;选项卡
1. 点按&#x200B;__获取服务凭据__&#x200B;按钮
1. 点按左上角的下载按钮，下载包含服务凭据值的JSON文件，并将文件保存到安全位置。
   + _如果服务凭据受到破坏，请立即联系Adobe支持以撤销它们_

## 安装服务凭据

服务凭据提供生成JWT所需的详细信息，JWT将交换为用AEM作为Cloud Service进行身份验证的访问令牌。 服务凭据必须存储在外部应用程序、系统或服务可访问的安全位置中，外部应用程序、系统或服务使用服务凭据访问AEM。 每个客户对服务凭据的管理方式和位置将是唯一的。

为了简单起见，本教程通过命令行将服务凭据传递给，但请与您的IT安全团队合作，了解如何根据组织的安全准则存储和访问这些凭据。

1. 将[下载的服务凭据JSON](#download-service-credentials)复制到项目根目录中名为`service_token.json`的文件
   + 但请记住，千万不要向Git提交任何凭据！

## 使用服务凭据

服务凭据（完全形成的JSON对象）与JWT和访问令牌不同。 相反，服务凭据（包含私钥）用于生成JWT，该JWT与Adobe IMS API交换以用于访问令牌。

![服务凭据 — 外部应用程序](assets/service-credentials/service-credentials-external-application.png)

1. 将服务凭据从AEM Developer Console下载到安全位置
1. 外部应用程序需要以编程方式与AEM交互，作为Cloud Service环境
1. 外部应用程序从安全位置读取服务凭据
1. 外部应用程序使用来自服务凭据的信息来构建JWT令牌
1. JWT令牌被发送到Adobe IMS以交换访问令牌
1. Adobe IMS返回一个访问令牌，可用来将AEM作为Cloud Service
   + 访问令牌可以请求过期。 最好保持访问令牌的生命短暂，并在需要时进行更新。
1. 外部应用程序将HTTP请求作为Cloud Service发送到AEM，并将访问令牌作为载体令牌添加到HTTP请求的授权标头
1. AEM作为Cloud Service接收HTTP请求、验证请求并执行HTTP请求所请求的工作，并将HTTP响应返回给外部应用程序

### 对外部应用程序的更新

要使用服务凭据作为Cloud Service访问AEM，必须通过3种方式更新外部应用程序：

1. 在服务凭据中读取
   + 为了简单起见，我们将从下载的JSON文件中读取这些内容，但在实际使用场景中，必须根据组织的安全准则安全地存储服务凭据
1. 从服务凭据生成JWT
1. 将JWT交换为访问令牌
   + 当存在服务凭据时，当将AEM作为Cloud Service访问时，我们的外部应用程序使用此访问令牌而不是本地开发访问令牌

在本教程中，Adobe的`@adobe/jwt-auth` npm模块用于两者，(1)从服务凭据生成JWT，(2)在单个函数调用中为访问令牌交换JWT。 如果您的应用程序不是基于JavaScript的，请查看其他语言](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)的[示例代码，了解如何从服务凭据创建JWT，并将其交换为使用Adobe IMS的访问令牌。

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

## 创建JWT并交换访问令牌

读取服务凭据后，它们用于生成JWT，然后与Adobe IMS API交换访问令牌，然后，JWT可用于作为Cloud Service访问AEM。

此示例应用程序基于Node.js，因此最好使用[@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) npm模块来促进(1)JWT生成和(20)与Adobe IMS交换。 如果您的应用程序是使用其他语言开发的，请查看[相应的代码示例](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)，了解如何使用其他编程语言构建到AdobeIMS的HTTP请求。

1. 更新`getAccessToken(..)`以检查JSON文件内容并确定它是表示本地开发访问令牌还是服务凭据。 可通过检查是否存在`.accessToken`属性(仅存于本地开发访问令牌JSON)来轻松实现此目标。

   如果提供了服务凭据，则应用程序生成JWT并与Adobe IMS交换JWT以用于访问令牌。 我们将使用[@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth)的`auth(...)`函数，该函数既生成JWT，又在单个函数调用中将其交换为访问令牌。  `auth(..)`的参数是一个[JSON对象，由服务凭据JSON中提供的特定信息](https://www.npmjs.com/package/@adobe/jwt-auth#config-object)组成，如代码中所述。

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

   现在，根据通过`file`命令行参数传入的JSON文件(本地开发访问令牌JSON或服务凭据JSON)，应用程序将派生访问令牌。

   请记住，当服务凭据未过期时，JWT和相应的访问令牌会过期，并且需要在它们过期之前进行刷新。 这可以通过使用AdobeIMS](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens)提供的`refresh_token` [来完成。

1. 在进行这些更改后，从AEM开发人员控制台下载的服务凭据JSON（为了简单起见，将其保存为与此`index.js`相同的文件夹`service_token.json`），执行将命令行参数`file`替换为`service_token.json`的应用程序，并将`propertyValue`更新为新值，以便在AEM中显示效果。

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Restricted Use" \
       file=service_token.json
   ```

   到终端的输出将如下：

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

   __403 - Forbidden__&#x200B;行指示作为Cloud Service对AEM的HTTP API调用中存在错误。 尝试更新资产的元数据时，会出现这些403禁止的错误。

   原因是服务凭据派生访问令牌使用自动创建的技术帐户AEM用户验证对AEM的请求，默认情况下，该用户仅具有读访问权限。 要向AEM提供应用程序写权限，必须在AEM中授予与访问令牌关联的技术帐户AEM用户权限。

## 在AEM中配置访问

服务凭据派生的访问令牌使用的是技术帐户AEM用户，该帐户具有参与者AEM用户组的成员资格。

![服务凭据 — 技术帐户AEM用户](./assets/service-credentials/technical-account-user.png)

技术帐户AEM用户在AEM中存在后(使用访问令牌的第一个HTTP请求后)，可以像管理其他AEM用户一样管理此AEM用户的权限。

1. 首先，打开从AEM Developer Console下载的服务凭据JSON，找到技术帐户的AEM登录名，然后找到`integration.email`值，该值应类似于：`12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`。
1. 以AEM管理员身份登录到相应AEM环境的作者服务
1. 导航到&#x200B;__工具__ > __安全__ > __用户__
1. 找到步骤1中标识的&#x200B;__登录名__&#x200B;的AEM用户，并打开其&#x200B;__属性__
1. 导航到&#x200B;__组__&#x200B;选项卡，然后添加&#x200B;__DAM用户__&#x200B;组（作为对资产的写入访问权限）
1. 点按&#x200B;__保存并关闭__

在AEM中拥有对资产具有写入权限的技术帐户后，请重新运行应用程序：

```shell
$ node index.js \
    aem=https://author-p1234-e5678.adobeaemcloud.com \
    folder=/wknd/en/adventures/napa-wine-tasting \
    propertyName=metadata/dc:rights \
    propertyValue="WKND Restricted Use" \
    file=service_token.json
```

到终端的输出将如下：

```
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
```

## 验证更改

1. 以已更新的Cloud Service环境身份登录AEM（使用`aem`命令行参数中提供的相同主机名）
1. 导航到&#x200B;__资产__ > __文件__
1. 导航到由`folder`命令行参数指定的资产文件夹，例如&#x200B;__WKND__ > __英语__ > __冒险__ > __纳帕品酒会__
1. 打开文件夹中任意资产的&#x200B;__属性__
1. 导航到&#x200B;__高级__&#x200B;选项卡
1. 查看已更新属性的值，例如映射到已更新`metadata/dc:rights` JCR属性的&#x200B;__Copyright__，该属性现在反映`propertyValue`参数中提供的值，例如&#x200B;__WKND Restricted Use__

![WKND受限使用元数据更新](./assets/service-credentials/asset-metadata.png)

## 恭喜！

现在，我们已使用本地开发访问令牌以编程方式将AEM作为Cloud Service访问，并使用生产就绪型服务到服务访问令牌!

