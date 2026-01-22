---
title: AEM作为云服务上的SAML 2.0
description: 了解如何在AEM上配置SAML 2.0认证作为云服务发布服务。
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
jira: KT-9351
thumbnail: 343040.jpeg
last-substantial-update: 2024-05-15T00:00:00Z
exl-id: 461dcdda-8797-4a37-a0c7-efa7b3f1e23e
duration: 2200
source-git-commit: 2ed303e316577363f6d1c265ef7f9cd6d81491d8
workflow-type: tm+mt
source-wordcount: '4277'
ht-degree: 1%

---

# SAML 2.0 认证{#saml-2-0-authentication}

学习如何设置和认证终端用户（非AEM作者）到你选择的SAML 2.0兼容IDP。

## AEM作为云服务的SAML是什么？

SAML 2.0 与 AEM Publish（或 Preview）集成，允许基于 AEM 的网页体验的终端用户向非 Adobe IDP（身份提供者）进行身份验证，并以指定授权用户身份访问 AEM。

|                       | AEM 作者 | AEM Publish |
|-----------------------|:----------:|:-----------:|
| SAML 2.0 支持 | ✘ | ✔ |

+++ 理解AEM的SAML 2.0流程

AEM Publish SAML 集成的典型流程如下：

1. 用户向AEM Publish发出请求，指示需要身份验证。
   + 用户请求受CUG/ACL保护的资源。
   + 用户请求受身份验证要求约束的资源。
   + 用户访问指向AEM登录端点（即`/system/sling/login`）的链接，该链接显式请求登录操作。
1. AEM向IDP发出AuthnRequest，请求IDP启动身份验证过程。
1. 用户认证为IDP。
   + 用户会被IDP提示输入凭证。
   + 用户已通过IDP认证，无需提供额外的凭证。
1. IDP 生成包含用户数据的 SAML 断言，并使用 IDP 的私有证书进行签名。
1. IDP通过HTTP POST通过用户的网页浏览器（RESPECTIVE_PROTECTED_PATH/saml_login）将SAML断言发送到AEM Publish。
1. AEM Publish 接收 SAML 断言，并使用 IDP 公共证书验证 SAML 断言的完整性和真实性。
1. AEM Publish 基于 SAML 2.0 OSGi 配置管理 AEM 用户记录，以及 SAML 断言的内容。
   + 创建用户
   + 同步用户属性
   + 更新AEM用户组成员资格
1. AEM Publish在HTTP响应中设置AEM `login-token` Cookie，用于向AEM Publish验证后续请求。
1. AEM Publish 会根据 cookie 指定 `saml_request_path` ，将用户重定向到 AEM Publish 上的 URL。

+++

## 配置演示

>[!VIDEO](https://video.tv.adobe.com/v/3455354?captions=chi_hans&quality=12&learn=on)

这个视频讲解了如何将SAML 2.0集成到AEM作为云服务发布服务，以及如何使用Okta作为IDP。

## 先决条件

设置SAML 2.0认证时需要以下条件：

+ 部署管理员对Cloud Manager的访问权限
+ AEM管理员访问AEM作为云服务环境
+ 管理员访问IDP
+ 可选地，可以访问用于加密SAML负载的公私钥对
+ AEM 网站页面（或页面树），发布到 AEM Publish，并 [由封闭用户组（CUG）保护](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/sites/authoring/sites-console/page-properties#permissions)

SAML 2.0 仅支持认证 AEM 发布或预览的使用。 若要使用和IDP管理AEM作者的身份验证，[请将IDP与Adobe IMS集成](https://helpx.adobe.com/cn/enterprise/using/set-up-identity.html)。


## 在AEM上安装IDP公共证书

IDP的公共证书将添加到AEM的全球信任存储区，并用于验证IDP发送的SAML断言是否有效。

+++SAML断言签名流程

![SAML 2.0 - IDP SAML断言签名](./assets/saml-2-0/idp-signing-diagram.png)

1. 用户向IDP进行身份验证。
1. IDP生成包含用户数据的SAML断言。
1. IDP使用IDP的私有证书签署SAML声明。
1. IDP向AEM发布的SAML端点(`.../saml_login`)发起客户端HTTP POST，该端点包括已签名的SAML断言。
1. AEM Publish接收包含已签名SAML声明的HTTP POST，可以使用IDP公共证书验证签名。

+++

![将IDP公共证书添加到全局信任存储区](./assets/saml-2-0/global-trust-store.png)

1. 从IDP获取 __公开证书__ 文件。 该证书允许 AEM 验证 IDP 向 AEM 提供的 SAML 断言。

   证书为PEM格式，应类似于：

   ```
   -----BEGIN CERTIFICATE-----
   MIIC4jCBAcoCCQC33wnybT5QZDANBgkqhkiG9w0BAQsFADAyMQswCQYDVQQGEwJV
   ...
   m0eo2USlSRTVl7QHRTuiuSThHpLKQQ==
   -----END CERTIFICATE-----
   ```

1. 以 AEM 管理员身份登录 AEM Author。
1. 访问 __“安全工具>信托商店__>”。
1. 创建或开设全球信托商店。 如果创建全球信任存储，请将密码存放在安全的地方。
1. 展开&#x200B;__从CER文件__&#x200B;添加证书。
1. 选择 __“选择证书文件__”，并上传IDP提供的证书文件。
1. 将地图证书留 __空给用户__ 。
1. 选择&#x200B;__提交__。
1. 新添加的证书显示在“ __从CRT文件__ 添加证书”部分之上。
1. 请注意别 __名__，因为该值用于 [SAML 2.0认证处理程序OSGi配置](#saml-2-0-authentication-handler-osgi-configuration)。
1. 选择&#x200B;__保存并关闭__。

全局信任存储在 AEM Author 上配置了 IDP 的公开证书，但由于 SAML 仅在 AEM Publish 上使用，必须将全局信任存储复制到 AEM Publish，IDP 公开证书才能访问。

![将全球信任存储复制到 AEM Publish](./assets/saml-2-0/global-trust-store-replicate.png)

1. 导航到 __工具>部署 > 包__。
1. 创建一个包
   + 包裹名称： `Global Trust Store`
   + 版本： `1.0.0`
   + 小组： `com.your.company`
1. 编辑新的 __全球信托存储__ 套餐。
1. 选择 __“过滤器”__ 标签，添加根路径 `/etc/truststore`的过滤器。
1. 选择 __完成__ ，然后选择 __保存__。
1. 为&#x200B;__全局信任存储区__&#x200B;包选择&#x200B;__生成__&#x200B;按钮。
1. 构建后，选择&#x200B;__更多__ > __复制__&#x200B;以将全局信任存储节点(`/etc/truststore`)激活到AEM发布。

## 创建认证服务密钥库{#authentication-service-keystore}

_当[SAML 2.0身份验证处理程序OSGi配置属性`handleLogout`设置为`true`](#saml-20-authenticationsaml-2-0-authentication)或需要[AuthnRequest签名/SAML断言加密](#install-aem-public-private-key-pair)时，需要为身份验证服务创建密钥库_

1. 以 AEM 管理员身份登录 AEM 作者，上传私钥。
1. 进入 __“安全>用户__>工具”，选择 __认证-服务__ 用户，从顶部作栏选择 __属性__ 。
1. 选择“ __钥匙库__ ”标签。
1. 创建或打开密钥存储器。 如果创建密钥存储，请妥善保管密码。
   + [只有在需要 AuthnRequest 签名/SAML 断言加密时，才会在该密钥库](#install-aem-public-private-key-pair)中安装公私钥库。
   + 如果该SAML集成支持登出，但不支持AuthnRequest签名/SAML断言，那么空密钥库就足够了。
1. 选择&#x200B;__保存并关闭__。
1. 创建包含更新的&#x200B;__authentication-service__&#x200B;用户的包。

   使用包使用以下临时解决方法(_U):_

   1. 导航到&#x200B;__工具>部署>包__。
   1. 创建资源包
      + 包裹名称： `Authentication Service`
      + 版本： `1.0.0`
      + 组： `com.your.company`
   1. 编辑新的&#x200B;__身份验证服务密钥存储__&#x200B;包。
   1. 选择&#x200B;__筛选器__&#x200B;选项卡，并为根路径`/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore`添加筛选器。
      + 导航到`<AUTHENTICATION SERVICE UUID>`工具>安全>用户&#x200B;__并选择__&#x200B;身份验证服务&#x200B;__用户，即可找到__。 UUID是URL的最后一部分。
   1. 选择 __完成__ ，然后选择 __保存__。
   1. 选择&#x200B;__认证服务密钥存储__&#x200B;包的&#x200B;__构建__&#x200B;按钮。
   1. 生成后，选择&#x200B;__更多__ > __复制__&#x200B;以激活AEM发布的身份验证服务密钥存储。

## 安装AEM公钥/私钥对{#install-aem-public-private-key-pair}

_安装AEM公钥/私钥对是可选的_

AEM Publish可以配置为对AuthnRequests（到IDP）进行签名，并对SAML断言(到AEM)进行加密。 这是通过提供专用密钥到AEM Publish来实现的，并且可将公共密钥与IDP进行匹配。

+++ 了解AuthnRequest签名流程（可选）

AuthnRequest(从AEM Publish向IDP发出的启动登录过程的请求)可由AEM Publish签名。 为此，AEM Publish使用私钥对AuthnRequest签名，然后IDP使用公钥验证签名。 这向IDP保证AuthnRequest是由AEM Publish发起和请求的，而不是恶意的第三方。

![SAML 2.0 - SP AuthnRequest签名](./assets/saml-2-0/sp-authnrequest-signing-diagram.png)

1. 用户向AEM Publish发出HTTP请求，从而向IDP发出SAML身份验证请求。
1. AEM Publish会生成要发送到IDP的SAML请求。
1. AEM Publish使用AEM的私钥对SAML请求进行签名。
1. AEM Publish会启动AuthnRequest，这是一个HTTP客户端重定向，指向包含已签名SAML请求的IDP。
1. IDP接收AuthnRequest，并使用AEM的公共密钥验证签名，从而确保AEM Publish启动了AuthnRequest。
1. AEM Publish 随后使用 IDP 公共证书验证解密后的 SAML 断言的完整性和真实性。

+++

+++ 理解SAML断言加密流程（可选）

所有 IDP 与 AEM Publish 之间的 HTTP 通信都应通过 HTTPS 进行，因此默认是安全的。 然而，根据需要，如果需要额外的保密性，SAML断言可以被加密，而非HTTPS提供的保密性。 为此，IDP使用私钥加密SAML断言数据，AEM Publish则使用私钥解密SAML断言。

![SAML 2.0 - SP SAML 断言加密](./assets/saml-2-0/sp-samlrequest-encryption-diagram.png)

1. 用户认证为IDP。
1. IDP生成包含用户数据的SAML声明，并使用IDP的私有证书对其进行签名。
1. IDP随后用AEM的公钥加密SAML断言，这需要AEM私钥来解密。
1. 加密的 SAML 断言通过用户的网页浏览器发送到 AEM Publish。
1. AEM Publish 接收 SAML 断言，并使用 AEM 的私钥解密。
1. IDP会提示用户进行身份验证。

+++

AuthnRequest 签名和 SAML 断言加密都是可选的，但它们都启用了，使用 [SAML 2.0 认证处理程序的 OSGi 配置属性 `useEncryption`](#saml-20-authenticationsaml-2-0-authentication)，意味着两者或都不能使用。

![AEM 认证服务密钥存储](./assets/saml-2-0/authentication-service-key-store.png)

1. 获取用于签署AuthnRequest的公钥、私钥（DER格式的PKCS#8）和证书链文件（这可能是公钥），并加密SAML断言。 密钥通常由IT组织的安全团队提供。

   + 自 __签名密钥对可以通过 openssl__ 生成：

   ```
   $ openssl req -x509 -sha256 -days 365 -newkey rsa:4096 -keyout aem-private.key -out aem-public.crt
   
   # Provide a password (keep in safe place), and other requested certificate information
   
   # Convert the keys to AEM's required format 
   $ openssl rsa -in aem-private.key -outform der -out aem-private.der
   $ openssl pkcs8 -topk8 -inform der -nocrypt -in aem-private.der -outform der -out aem-private-pkcs8.der
   ```

1. 把公钥上传到IDP。
   + 使用`openssl`上述方法，公钥即为文件。`aem-public.crt`
1. 以 AEM 管理员身份登录 AEM 作者，上传私钥。
1. 进入 __信任商店__>安全工具>，选择 __认证服务__ 用户，然后从顶部作栏选择 __属性__ 。
1. 进入 __“安全>用户__>工具”，选择 __认证-服务__ 用户，从顶部作栏选择 __属性__ 。
1. 选择“ __钥匙库__ ”标签。
1. 创建或打开密钥存储器。 如果创建密钥存储，请妥善保管密码。
1. 选择&#x200B;__从DER文件添加私钥__，并将私钥和链文件添加到AEM：
   + __别名__：提供有意义的名称，通常是IDP的名称。
   + __私钥文件__：上载私钥文件（DER格式为PKCS#8）。
      + 使用上述`openssl`方法，这是`aem-private-pkcs8.der`文件
   + __选择证书链文件__：上传配套的链文件（这可能是公钥）。
      + 按照`openssl`上面的方法，这就是文件`aem-public.crt`
   + 选择&#x200B;__提交__
1. 新添加的证书显示在&#x200B;__从CRT文件__&#x200B;添加证书部分上方。
   + 记下&#x200B;__别名__，因为该别名在[SAML 2.0身份验证处理程序OSGi配置](#saml-20-authentication-handler-osgi-configuration)中使用
1. 选择&#x200B;__保存并关闭__。
1. 创建一个包含更新 __后的认证服务__ 用户的包。

   _Use以下使用包的临时变通方法&#x200B;:_

   1. 导航到 __工具>部署 > 包__。
   1. 创建一个包
      + 包名称： `Authentication Service`
      + 版本： `1.0.0`
      + 组： `com.your.company`
   1. 编辑新的&#x200B;__身份验证服务密钥存储__&#x200B;包。
   1. 选择&#x200B;__筛选器__&#x200B;选项卡，并为根路径`/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore`添加筛选器。
      + 导航到`<AUTHENTICATION SERVICE UUID>`工具>安全>用户&#x200B;__并选择__&#x200B;身份验证服务&#x200B;__用户，即可找到__。 UUID是URL的最后一部分。
   1. 选择&#x200B;__完成__，然后选择&#x200B;__保存__。
   1. 选择&#x200B;__认证服务密钥存储__&#x200B;包的&#x200B;__构建__&#x200B;按钮。
   1. 构建完成后，选择 __更多__ > __复制__ 以激活认证服务密钥库至AEM Publish。

## 配置SAML 2.0认证处理器{#configure-saml-2-0-authentication-handler}

AEM的SAML配置通过 __Adobe Granite SAML 2.0认证处理__ 程序OSGi配置实现。该配置是OSGi工厂配置，这意味着单个AEM as a Cloud Service Publish服务可能有多个SAML配置，其涵盖存储库的离散资源树；这对于多站点AEM部署很有用。

+++ SAML 2.0身份验证处理程序OSGi配置术语表

### Adobe Granite SAML 2.0身份验证处理程序OSGi配置{#configure-saml-2-0-authentication-handler-osgi-configuration}

|                                   | OSGi属性 | 必需 | 值格式 | 默认值 | 描述 |
|-----------------------------------|-------------------------------|:--------:|:---------------------:|---------------------------|-------------|
| 路径 | `path` | ✔ | 字符串数组 | `/` | 此身份验证处理程序用于的AEM路径。 |
| IDP URL | `idpUrl` | ✔ | 字符串 |                           | 发送 IDP URL 以及 SAML 认证请求。 |
| IDP证书别名 | `idpCertAlias` | ✔ | 字符串 |                           | 在AEM全球信托存储中发现的IDP证书别名 |
| IDP HTTP 重定向 | `idpHttpRedirect` | ✘ | 布尔值 | `false` | 表示是否HTTP 重定向到 IDP URL，而不是发送 AuthnRequest。 设置为 用于 `true` IDP发起的认证。 |
| IDP标识符 | `idpIdentifier` | ✘ | 字符串 |                           | 唯一的IDP ID以确保AEM用户和组的独特性。 如果空，则使用 。`serviceProviderEntityId` |
| 断言消费者服务网址 | `assertionConsumerServiceURL` | ✘ | 字符串 |                           | `AssertionConsumerServiceURL` AuthnRequest 中的 URL 属性，指定消息必须发送到 AEM 的地点`<Response>`。 |
| SP实体Id | `serviceProviderEntityId` | ✔ | 字符串 |                           | 唯一标识AEM与IDP相关;通常是 AEM 主机名。 |
| SP加密 | `useEncryption` | ✘ | 布尔值 | `true` | 指示IDP是否加密SAML断言。 需要设置`spPrivateKeyAlias`和`keyStorePassword`。 |
| SP私钥别名 | `spPrivateKeyAlias` | ✘ | 字符串 |                           | 私钥在用户密钥存储中的 `authentication-service` 别名。 如果`useEncryption`设置为`true`，则此为必填字段。 |
| SP密钥存储密码 | `keyStorePassword` | ✘ | 字符串 |                           | “authentication-service”用户的密钥存储的密码。 如果`useEncryption`设置为`true`，则此为必填字段。 |
| 默认重定向 | `defaultRedirectUrl` | ✘ | 字符串 | `/` | 成功身份验证后的默认重定向URL。 可以相对于AEM主机（例如，`/content/wknd/us/en/html`）。 |
| 用户ID属性 | `userIDAttribute` | ✘ | 字符串 | `uid` | SAML断言属性的名称，包含AEM用户的用户ID。 保持空以使用。`Subject:NameId` |
| 自动创建 AEM 用户 | `createUser` | ✘ | 布尔值 | `true` | 指示是否在成功身份验证时创建AEM用户。 |
| AEM用户中间路径 | `userIntermediatePath` | ✘ | 字符串 |                           | 创建AEM用户时，此值用作中间路径（例如，`/home/users/<userIntermediatePath>/jane@wknd.com`）。 需要将`createUser`设置为`true`。 |
| AEM用户属性 | `synchronizeAttributes` | ✘ | 字符串数组 |                           | 要存储在AEM用户上的SAML属性映射列表，格式为`[ "saml-attribute-name=path/relative/to/user/node" ]`（例如`[ "firstName=profile/givenName" ]`）。 查看 [完整的本地 AEM 属性](#aem-user-attributes)列表。 |
| 将用户添加到AEM组 | `addGroupMemberships` | ✘ | 布尔值 | `true` | 指示在成功身份验证后是否自动将AEM用户添加到AEM用户组。 |
| AEM组成员资格属性 | `groupMembershipAttribute` | ✘ | 字符串 | `groupMembership` | SAML断言属性的名称，该属性包含应将该用户添加到的AEM用户组的列表。 需要将`addGroupMemberships`设置为`true`。 |
| 默认AEM组 | `defaultGroups` | ✘ | 字符串数组 |                           | 已验证身份的用户的AEM用户组列表始终添加到（例如，`[ "wknd-user" ]`）。 需要将`addGroupMemberships`设置为`true`。 |
| NameIDPolicy格式 | `nameIdFormat` | ✘ | 字符串 | `urn:oasis:names:tc:SAML:2.0:nameid-format:transient` | 在AuthnRequest消息中发送的NameIDPolicy格式参数的值。 |
| 存储SAML响应 | `storeSAMLResponse` | ✘ | 布尔值 | `false` | 指示`samlResponse`值是否存储在AEM `cq:User`节点上。 |
| 处理登出 | `handleLogout` | ✘ | 布尔值 | `false` | 表示登出请求是否由该SAML认证处理器处理。 需要 `logoutUrl` 设置。 |
| 登出网址 | `logoutUrl` | ✘ | 字符串 |                           | 发送SAML注销请求的IDP URL。 如果 `handleLogout` 设置为 `true`，则为必需。 |
| 时钟公差 | `clockTolerance` | ✘ | 整数 | `60` | 验证SAML断言时，IDP和AEM (SP)时钟偏差容错。 |
| 摘要方法 | `digestMethod` | ✘ | 字符串 | `http://www.w3.org/2001/04/xmlenc#sha256` | IDP在签名SAML消息时使用的摘要算法。 |
| 签名方法 | `signatureMethod` | ✘ | 字符串 | `http://www.w3.org/2001/04/xmldsig-more#rsa-sha256` | IDP在签署SAML消息时使用的签名算法。 |
| 身份同步类型 | `identitySyncType` | ✘ | `default` 或 `idp` | `default` | 请勿更改AEM as a Cloud Service的`from`默认值。 |
| 服务排名 | `service.ranking` | ✘ | 整数 | `5002` | 对于相同的 `path`，优先考虑更高的排名配置。 |

### AEM 用户属性{#aem-user-attributes}

AEM使用以下用户属性，这些属性可通过Adobe Granite SAML 2.0身份验证处理程序OSGi配置中的`synchronizeAttributes`属性填充。  任何IDP属性都可以同步到任何AEM用户属性，但是，映射到AEM使用属性属性（如下所列）允许AEM自然使用这些属性。

| 用户属性 | 来自`rep:User`节点的相对属性路径 |
|--------------------------------|--------------------------|
| 标题（例如，`Mrs`） | `profile/title` |
| 名字（即名字） | `profile/givenName` |
| 姓氏（即姓氏） | `profile/familyName` |
| 职务 | `profile/jobTitle` |
| 电子邮件地址 | `profile/email` |
| 街道地址 | `profile/street` |
| 城市 | `profile/city` |
| 邮政编码 | `profile/postalCode` |
| 国家/地区 | `profile/country` |
| 电话号码 | `profile/phoneNumber` |
| 自我介绍 | `profile/aboutMe` |

+++

1. 在`/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json`的项目中创建一个OSGi配置文件，并在IDE中打开。
   + 将`/wknd-examples/`更改为您的`/<project name>/`
   + 文件名中`~`之后的标识符应唯一标识此配置，因此它可能是IDP的名称，如`...~okta.cfg.json`。 该值应为带连字符的字母数字。
1. 将以下JSON粘贴到`com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json`文件中，并根据需要更新`wknd`引用。

   ```json
   {
       "path": [ "/content/wknd", "/content/dam/wknd" ], 
       "idpCertAlias": "$[env:SAML_IDP_CERT_ALIAS;default=certalias___1652125559800]",
       "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/exk4z55r44Jz9C6am5d7]",
       "idpUrl": "$[env:SAML_IDP_URL;default=https://dev-5511372.okta.com/app/dev-5511372_aemasacloudservice_1/exk4z55r44Jz9C6am5d7/sso/saml]",
       "serviceProviderEntityId": "$[env:SAML_AEM_ID;default=https://publish-p123-e456.adobeaemcloud.com]",
       "useEncryption": false,
       "createUser": true,
       "userIntermediatePath": "wknd/idp",
       "synchronizeAttributes":[
           "firstName=profile/givenName"
       ],
       "addGroupMemberships": true,
       "defaultGroups": [ 
           "wknd-users"
       ]
   }
   ```

1. 根据项目要求更新值。 有关配置属性说明，请参阅上面的&#x200B;__SAML 2.0身份验证处理程序OSGi配置术语表__。 `path`应包含受封闭用户组(CUG)保护且需要身份验证的内容树，此身份验证处理程序应负责保护。
1. 建议但非强制，当值可能与发布周期不同步，或相似环境类型/服务层之间的值不同时，使用OSGi环境变量和秘密。 默认值可以通过上述语法设置 `$[env:..;default=the-default-value]"` 。

如果SAML配置在不同环境中不同，每个环境（`config.publish.dev`、 `config.publish.stage`和 `config.publish.prod`）的OSGi配置可以用特定属性定义。

### 使用加密

在[加密AuthnRequest和SAML断言](#encrypting-the-authnrequest-and-saml-assertion)时，需要以下属性： `useEncryption`、`spPrivateKeyAlias`和`keyStorePassword`。 `keyStorePassword`包含密码，因此不能将该值存储在OSGi配置文件中，而是使用[机密配置值](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=zh-Hans#secret-configuration-values)插入

+++（可选）更新OSGi配置以使用加密

1. 在IDE中打开`/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json`。
1. 添加三个属性`useEncryption`、`spPrivateKeyAlias`和`keyStorePassword`，如下所示。

   ```json
   {
   "path": [ "/content/wknd", "/content/dam/wknd" ], 
   "idpCertAlias": "$[env:SAML_IDP_CERT_ALIAS;default=certalias___1234567890]",
   "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/abcdef1235678]",
   "idpUrl": "$[env:SAML_IDP_URL;default=https://dev-5511372.okta.com/app/dev-123567890_aemasacloudservice_1/abcdef1235678/sso/saml]",
   "serviceProviderEntityId": "$[env:SAML_AEM_ID;default=https://publish-p123-e456.adobeaemcloud.com]",
   "useEncryption": true,
   "spPrivateKeyAlias": "$[env:SAML_AEM_KEYSTORE_ALIAS;default=aem-saml-encryption]",
   "keyStorePassword": "$[secret:SAML_AEM_KEYSTORE_PASSWORD]",
   "createUser": true,
   "userIntermediatePath": "wknd/idp"
   "synchronizeAttributes":[
       "firstName=profile/givenName"
   ],
   "addGroupMemberships": true,
   "defaultGroups": [ 
       "wknd-users"
   ]
   }
   ```

1. 加密所需的三个OSGi配置属性包括：

+ `useEncryption`设置为`true`
+ `spPrivateKeyAlias`包含SAML集成使用的私钥的密钥库条目别名。
+ `keyStorePassword`包含包含[用户密钥库密码的](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=zh-Hans#secret-configuration-values)OSGi密码配置变量`authentication-service`。

+++

## 配置反向链接筛选条件

在SAML身份验证过程中，IDP向AEM Publish的`.../saml_login`端点发起客户端HTTP POST。 如果IDP和AEM发布存在于其他来源，则AEM发布的&#x200B;__反向链接筛选器__&#x200B;将通过OSGi配置配置为允许来自IDP来源的HTTP POST。

1. 在`/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/org.apache.sling.security.impl.ReferrerFilter.cfg.json`处的项目中创建（或编辑）OSGi配置文件。
   + 将`/wknd-examples/`更改为您的`/<project name>/`
1. 请确保`allow.empty`值设置为`true`，`allow.hosts` （或者，如果您愿意，`allow.hosts.regexp`）包含IDP的来源，`filter.methods`包含`POST`。 OSGi配置应类似于：

   ```json
   {
       "allow.empty": true,
       "allow.hosts.regexp": [ ],
       "allow.hosts": [ 
           "$[env:SAML_IDP_REFERRER;default=dev-123567890.okta.com]"
       ],
       "filter.methods": [
           "POST",
       ],
       "exclude.agents.regexp": [ ]
   }
   ```

AEM Publish支持单个反向链接过滤器配置，因此请将SAML配置要求与任何现有配置合并。

如果环境（或`config.publish.dev`）在不同环境中不同，`config.publish.stage`OSGi配置`config.publish.prod` `allow.hosts` `allow.hosts.regex`（、和）可以定义为特定属性。

## 配置跨源资源共享（CORS）

在SAML认证过程中，IDP会向AEM Publish的 `.../saml_login` 端点发起客户端HTTP POST。 如果IDP和AEM Publish存在于不同的主机/域上，AEM Publish的 __CRoss-Origin资源共享（CORS）__ 必须配置为允许来自IDP主机/域的HTTP POST。

该HTTP POST请求的 `Origin` 头通常与AEM发布主机的值不同，因此需要CORS配置。

在本地AEM SDK (`localhost:4503`)上测试SAML身份验证时，IDP可能会将`Origin`标头设置为`null`。 如果是，请补充`"null"` `alloworigin`。

1. 在`/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~saml.cfg.json`处的项目中创建一个OSGi配置文件
   + 将`/wknd-examples/`更改为您的项目名称
   + 文件名中`~`之后的标识符应唯一标识此配置，因此它可能是IDP的名称，如`...CORSPolicyImpl~okta.cfg.json`。 该值应为带连字符的字母数字。
1. 将以下 JSON 粘贴到文件中 `com.adobe.granite.cors.impl.CORSPolicyImpl~...cfg.json` 。

```json
{
    "alloworigin": [ 
        "$[env:SAML_IDP_ORIGIN;default=https://dev-1234567890.okta.com]", 
        "null"
    ],
    "allowedpaths": [ 
        ".*/saml_login"
    ],
    "supportedmethods": [ 
        "POST"
    ]
}
```

每个环境（`config.publish.dev`、 `config.publish.stage`和 `config.publish.prod`）的OSGi配置可以定义，如果 和`alloworigin` `allowedpaths`在不同环境中存在差异，则可定义特定属性。

## 配置AEM Dispatcher以允许SAML HTTP POST发送

在成功认证IDP后，IDP将向AEM注册 `/saml_login` 端点（在IDP中配置）编排HTTP POST。 该HTTP POST `/saml_login` 默认在Dispatcher被阻挡，因此必须通过以下Dispatcher规则明确允许：

1. 在你的IDE中打开 `dispatcher/src/conf.dispatcher.d/filters/filters.any` 。
1. 在文件底部添加一个允许规则，用于向以`/saml_login`结尾的URL发送HTTP POST。

```
...

# Allow SAML HTTP POST to ../saml_login end points
/0190 { /type "allow" /method "POST" /url "*/saml_login" }
```

>[!NOTE]
>在AEM中为各种受保护路径和不同IDP端点部署多个SAML配置时，请确保IDP向ASIVENT_PROTECTED_PATH/saml_login端点发布内容，以在AEM端选择适当的SAML配置。 如果同一受保护路径存在重复的SAML配置，则将随机选择SAML配置。

如果在Apach网页服务器上配置了URL重写（`dispatcher/src/conf.d/rewrites/rewrite.rules`），确保发送到 `.../saml_login` 端点的请求不会被意外破坏。

## 动态组成员

动态组成员资格是[Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/docs/security/authentication/external/dynamic.html)中的一项功能，可提升组评估和设置的性能。 本节介绍启用此功能时如何存储用户和组，以及如何修改SAML身份验证处理程序的配置，以便为新环境或现有环境启用它。

### 如何为新环境中的SAML用户启用动态组成员资格

为了显着提升新AEM as a Cloud Service环境中的群组评估性能，建议在新环境中激活动态群组成员资格功能。
这也是激活数据同步时的必要步骤。 更多详细信息[此处](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier) 。
为此，请将以下属性添加到OSGI配置文件中：

`/apps/example/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~example.cfg.json`

使用此配置，用户和组将创建为[Oak外部用户](https://jackrabbit.apache.org/oak/docs/security/authentication/identitymanagement.html)。 在AEM中，外部用户和组具有由`rep:principalName`或`[user name];[idp]`组成的默认`[group name];[idp]`。
请注意，访问控制列表(ACL)与用户或组的PrincipalName相关联。
在现有部署中部署此配置时，如果先前的`identitySyncType`未指定或设置为`default`，则将创建新用户和组，并且必须将ACL应用于这些新用户和组。 请注意，外部组不能包含本地用户。 [Repoinit](https://sling.apache.org/documentation/bundles/repository-initialization.html)可用于为SAML外部组创建ACL，即使仅在用户执行登录时创建它们。
为避免对ACL进行此重构，已实施标准[迁移功能](#automatic-migration-to-dynamic-group-membership-for-existing-environments)。

### 成员如何存储在具有动态组成员的本地和外部组中

在本地组中，组成员存储在 oak 属性中： `rep:members`。 该属性包含了该组每个成员的 uid 列表。 更多详情请见 [此处](https://jackrabbit.apache.org/oak/docs/security/user/membership.html#member-representation-in-the-repository)。示例：

```
{
  "jcr:primaryType": "rep:Group",
  "rep:principalName": "operators",
  "rep:managedByIdp": "SAML",
  "rep:members": [
    "635afa1c-beeb-3262-83c4-38ea31e5549e",
    "5e496093-feb6-37e9-a2a1-7c87b1cec4b0",
    ...
  ],
   ...
}
```

具有动态组成员的外部组不会在组条目中存储任何成员。组成员身份则存储在用户条目中。 其他文档可在[此处](https://jackrabbit.apache.org/oak/docs/security/authentication/external/dynamic.html)找到。 例如，这是该组的OAK节点：

```
{
  "jcr:primaryType": "rep:Group",
  "jcr:mixinTypes": [
    "rep:AccessControllable"
  ],
  "jcr:createdBy": "",
  "jcr:created": "Tue Jul 16 2024 08:58:47 GMT+0000",
  "rep:principalName": "GROUP_1;aem-saml-idp-1",
  "rep:lastSynced": "Tue Jul 16 2024 08:58:47 GMT+0000",
  "jcr:uuid": "d9c6af8a-35c0-3064-899a-59af55455cd0",
  "rep:externalId": "GROUP_1;aem-saml-idp-1",
  "rep:authorizableId": "GROUP_1;aem-saml-idp-1"
}
```

这是该组的用户成员的节点：

```
{
  "jcr:primaryType": "rep:User",
  "jcr:mixinTypes": [
    "rep:AccessControllable"
  ],
  "surname": "Test",
  "rep:principalName": "testUser",
  "rep:externalId": "test;aem-saml-idp-1",
  "rep:authorizableId": "test",
  "rep:externalPrincipalNames": [
    "projects-users;aem-saml-idp-1",
    "GROUP_2;aem-saml-idp-1",
    "GROUP_1;aem-saml-idp-1",
    "operators;aem-saml-idp-1"
  ],
  ...
}
```

### 如何在现有环境中为SAML用户启用动态组成员资格

如上一节中所述，外部用户和组的格式与本地用户和组使用的格式略有不同。 可以为外部组定义新的ACL并配置新的外部用户，也可以使用如下所述的迁移工具。

#### 为具有外部用户的现有环境启用动态组成员资格

SAML身份验证处理程序在指定以下属性时创建外部用户： `"identitySyncType": "idp"`。 在这种情况下，可以启用动态组成员资格，以便将此属性修改为： `"identitySyncType": "idp_dynamic"`。 无需迁移。

#### 自动迁移到具有本地用户的现有环境的动态组成员

当指定以下属性时，SAML 认证处理程序会创建本地用户： `"identitySyncType": "default"`。 当该属性未被指定时，这也是默认值。 本节介绍自动迁移程序执行的步骤。

启用此迁移后，将在用户身份验证期间执行，包括以下步骤：
1. 本地用户会迁移到外部用户，同时保留原用户名。 这意味着迁移的本地用户，作为外部用户，保留了原用户名，而不是遵循前文提到的命名语法。 还会添加一个额外的属性，称为： `rep:externalId` ，其值为 `[user name];[idp]`。 用户 `PrincipalName` 未被修改。
2. 对于SAML断言中收到的每个外部组，都会创建一个外部组。 如果存在对应的局部群，则将外部群作为成员加入该本地群。
3. 用户被添加为外部组的成员。
4. 本地用户随后会被从他所属的所有Saml本地群组中移除。 Saml局部群由OAK属性标识： `rep:managedByIdp`。 当属性 `syncType` 未指定或未设置为 `default`时，Saml认证处理程序会设置该属性。

例如，如果迁移前 `user1` 是本地用户，是本地组 `group1`成员，迁移后将发生以下变化：
`user1` 成为外部用户。 该属性 `rep:externalId` 被添加到他的个人资料中。`user1`成为外部群的成员：`group1;idp` `user1`不再是局部群的直接成员：`group1` `group1;idp`是局部群的成员。 `group1`

`user1` 则是局部群的成员： `group1` 尽管继承

外部组的组成员资格存储在属性`rep:externalPrincipalNames`的用户配置文件中

### 如何配置自动迁移到动态组成员资格

1. 在SAML OSGi配置文件`"identitySyncType": "idp_dynamic_simplified_id"`中启用属性`com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json`：
2. 使用工厂PID配置新的OSGi服务，其开始值为： `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration~`。 例如，PID可以是： `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration~myIdP`。 设置以下属性：

```
{
  "idpIdentifier": "<value of IDP Identifier (idpIdentifier)" property from the "com.adobe.granite.auth.saml.SamlAuthenticationHandler" configuration to be migrated>"
}
```

要迁移多个 SAML 配置，必须创建多个 的 OSGi 出厂配置 `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration` ，每个配置指定迁移对象 `idpIdentifier` 。

## 部署SAML配置

OSGi 配置必须提交到 Git，并通过 Cloud Manager 部署到 AEM 作为云服务。

```
$ git remote -v            
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (fetch)
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (push)
$ git add .
$ git commit -m "SAML 2.0 configurations"
$ git push adobe saml-auth:develop
```

使用全栈部署流水线部署目标云管理器 Git 分支（本例中为 `develop`），部署目标。

## 调用SAML认证

SAML 认证流程可以通过 AEM 网站网页调用，通过创建专门制作的链接或按钮。 下面描述的参数可以根据需要进行编程设置，因此，例如，登录按钮可以根据按钮的上下文将`saml_request_path`（成功SAML身份验证后用户在该处进行身份验证）设置到不同的AEM页面。

## 使用SAML时的安全缓存

在AEM发布实例上，通常都会缓存大多数页面。 但是，对于受SAML保护的路径，应使用auth_checker配置禁用缓存或启用安全缓存。 有关详细信息，请参阅[此处](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-dispatcher/using/configuring/permissions-cache)提供的详细信息

请注意，如果您在缓存受保护的路径时没有启用auth_checker，则可能会遇到无法预测的行为。

### GET请求

SAML 认证可以通过创建格式为 HTTP GET 的请求来调用：

`HTTP GET /system/sling/login`

并提供查询参数：

| 查询参数名称 | 查询参数值 |
|----------------------|-----------------------|
| `resource` | 任何作为SAML身份验证处理程序侦听的JCR路径或子路径，如[Adobe Granite SAML 2.0身份验证处理程序OSGi配置的](#configure-saml-2-0-authentication-handler) `path`属性中所定义。 |
| `saml_request_path` | 成功SAML身份验证后用户应采用的URL路径。 |

例如，此HTML链接将触发SAML登录流程，成功后将用户转到`/content/wknd/us/en/protected/page.html`。 可以根据需要以编程方式设置这些查询参数。

```html
<a href="/system/sling/login?resource=/content/wknd&saml_request_path=/content/wknd/us/en/protected/page.html">
    Log in using SAML
</a>
```

## POST 请求

可以通过创建以下格式的HTTP POST请求来调用SAML身份验证：

`HTTP POST /system/sling/login`

并提供表单数据：

| 表单数据名称 | 表单数据值 |
|----------------------|-----------------------|
| `resource` | 任何作为SAML身份验证处理程序侦听的JCR路径或子路径，如[Adobe Granite SAML 2.0身份验证处理程序OSGi配置的](#configure-saml-2-0-authentication-handler) `path`属性中所定义。 |
| `saml_request_path` | 成功SAML身份验证后用户应采用的URL路径。 |


例如，此HTML按钮将使用HTTP POST触发SAML登录流程，成功后，将用户带到`/content/wknd/us/en/protected/page.html`。 可以根据需要以编程方式设置这些表单数据参数。

```html
<form action="/system/sling/login" method="POST">
    <input type="hidden" name="resource" value="/content/wknd">
    <input type="hidden" name="saml_request_path" value="/content/wknd/us/en/protected/page.html">
    <input type="submit" value="Log in using SAML">
</form>
```

### Dispatcher 配置

HTTP GET和POST方法都需要客户端访问AEM的`/system/sling/login`端点，因此必须允许通过AEM Dispatcher访问它们。

根据是否使用 GET 或 POST ，允许必要的 URL 模式

```
# Allow GET-based SAML authentication invocation
/0191 { /type "allow" /method "GET" /url "/system/sling/login" /query "*" }

# Allow POST-based SAML authentication invocation
/0192 { /type "allow" /method "POST" /url "/system/sling/login" }
```
