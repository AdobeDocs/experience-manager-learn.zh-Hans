---
title: AEM as a Cloud Service上的SAML 2.0
description: 了解如何在AEM as a Cloud Service Publish服务上配置SAML 2.0身份验证。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9351
thumbnail: 343040.jpeg
last-substantial-update: 2024-05-15T00:00:00Z
exl-id: 461dcdda-8797-4a37-a0c7-efa7b3f1e23e
duration: 2200
source-git-commit: 87dd4873152d4690abb1efcfebd43d10033afa0a
workflow-type: tm+mt
source-wordcount: '3919'
ht-degree: 1%

---

# SAML 2.0身份验证{#saml-2-0-authentication}

了解如何为您选择的SAML 2.0兼容IDP设置和验证最终用户(而非AEM作者)。

## AEM as a Cloud Service的SAML是什么？

SAML 2.0与AEM Publish（或预览）集成，允许基于AEM的Web体验的最终用户向非AdobeIDP（身份提供程序）进行身份验证，并以已命名授权用户的身份访问AEM。

|                       | AEM Author | AEM 发布 |
|-----------------------|:----------:|:-----------:|
| SAML 2.0支持 | ✘ | ✔ |

+++ 了解使用AEM的SAML 2.0流程

AEM Publish SAML集成的典型流程如下所示：

1. 用户向AEM Publish发出请求，指示需要身份验证。
   + 用户请求受CUG/ACL保护的资源。
   + 用户请求受身份验证要求约束的资源。
   + 用户遵循指向AEM登录端点（即`/system/sling/login`）的链接，该链接显式请求登录操作。
1. AEM向IDP发出AuthnRequest，请求IDP启动身份验证过程。
1. 用户向IDP进行身份验证。
   + IDP会提示用户输入凭据。
   + 用户已经通过IDP的身份验证，无需提供进一步的凭据。
1. IDP生成包含用户数据的SAML声明，并使用IDP的私有证书对其进行签名。
1. IDP通过HTTPPOST，通过用户的Web浏览器，将SAML断言发送到AEM Publish。
1. AEM Publish接收SAML声明，并使用IDP公共证书验证SAML声明的完整性和真实性。
1. AEM Publish根据SAML 2.0 OSGi配置和SAML断言的内容管理AEM用户记录。
   + 创建用户
   + 同步用户属性
   + 更新AEM用户组成员资格
1. AEM Publish在HTTP响应上设置AEM `login-token` Cookie，用于向AEM Publish验证后续请求。
1. AEM Publish将用户重定向到`saml_request_path` Cookie指定的AEM Publish上的URL。

+++

## 配置演练

>[!VIDEO](https://video.tv.adobe.com/v/343040?quality=12&learn=on)

此视频介绍如何设置与AEM as a Cloud Service Publish服务的SAML 2.0集成，以及如何将Okta用作IDP。

## 先决条件

设置SAML 2.0身份验证时需要满足以下条件：

+ 部署管理员对Cloud Manager的访问权限
+ AEM管理员对AEM as a Cloud Service环境的访问权限
+ 对IDP的管理员访问权限
+ （可选）访问用于加密SAML有效负载的公钥/私钥对

仅支持SAML 2.0向AEM Publish或预览版验证用户。 若要使用和IDP管理AEM Author的身份验证，[请将IDP与Adobe IMS集成](https://helpx.adobe.com/cn/enterprise/using/set-up-identity.html)。


## 在AEM上安装IDP公共证书

IDP的公共证书将添加到AEM全局信任存储区，并用于验证IDP发送的SAML断言是否有效。

+++SAML断言签名流程

![SAML 2.0 - IDP SAML断言签名](./assets/saml-2-0/idp-signing-diagram.png)

1. 用户向IDP进行身份验证。
1. IDP生成包含用户数据的SAML断言。
1. IDP使用IDP的私有证书签署SAML声明。
1. IDP向AEM Publish的SAML端点(`.../saml_login`)发起客户端HTTPPOST，该端点包括已签名的SAML断言。
1. AEM Publish接收包含已签名SAML声明的HTTPPOST，可以使用IDP公共证书验证签名。

+++

![将IDP公共证书添加到全局信任存储区](./assets/saml-2-0/global-trust-store.png)

1. 从IDP获取&#x200B;__公共证书__&#x200B;文件。 该证书允许AEM验证IDP向AEM提供的SAML声明。

   证书为PEM格式，应类似于：

   ```
   -----BEGIN CERTIFICATE-----
   MIIC4jCBAcoCCQC33wnybT5QZDANBgkqhkiG9w0BAQsFADAyMQswCQYDVQQGEwJV
   ...
   m0eo2USlSRTVl7QHRTuiuSThHpLKQQ==
   -----END CERTIFICATE-----
   ```

1. 以AEM管理员身份登录AEM Author。
1. 导航到&#x200B;__工具>安全>信任存储区__。
1. 创建或打开全局信任存储区。 如果创建全局信任存储区，请将密码保存在安全的地方。
1. 展开&#x200B;__从CER文件__&#x200B;添加证书。
1. 选择&#x200B;__选择证书文件__，然后上传IDP提供的证书文件。
1. 将&#x200B;__将证书映射到用户__&#x200B;保留为空。
1. 选择&#x200B;__提交__。
1. 新添加的证书显示在&#x200B;__从CRT文件__&#x200B;添加证书部分上方。
1. 记下&#x200B;__别名__，因为该值在[SAML 2.0身份验证处理程序OSGi配置](#saml-2-0-authentication-handler-osgi-configuration)中使用。
1. 选择&#x200B;__保存并关闭__。

全局信任存储区在AEM Author上使用IDP的公共证书进行配置，但由于SAML仅在AEM Publish上使用，因此必须将全局信任存储区复制到AEM Publish，才能在其中访问IDP公共证书。

![将全局信任存储区复制到AEM Publish](./assets/saml-2-0/global-trust-store-replicate.png)

1. 导航到&#x200B;__工具>部署>包__。
1. 创建资源包
   + 包名称： `Global Trust Store`
   + 版本： `1.0.0`
   + 组： `com.your.company`
1. 编辑新的&#x200B;__全局信任存储区__&#x200B;包。
1. 选择&#x200B;__筛选器__&#x200B;选项卡，并为根路径`/etc/truststore`添加筛选器。
1. 选择&#x200B;__完成__，然后选择&#x200B;__保存__。
1. 为&#x200B;__全局信任存储区__&#x200B;包选择&#x200B;__生成__&#x200B;按钮。
1. 生成后，选择&#x200B;__更多__ > __复制__&#x200B;以激活全局信任存储节点(`/etc/truststore`)到AEM Publish。

## 创建认证服务密钥库{#authentication-service-keystore}

_当[SAML 2.0身份验证处理程序OSGi配置属性`handleLogout`设置为`true`](#saml-20-authenticationsaml-2-0-authentication)或需要[AuthnRequest签名/SAML断言加密](#install-aem-public-private-key-pair)时，需要为身份验证服务创建密钥库_

1. 以AEM管理员身份登录AEM Author以上传私钥。
1. 导航到&#x200B;__工具>安全>用户__，选择&#x200B;__身份验证服务__&#x200B;用户，然后从顶部操作栏中选择&#x200B;__属性__。
1. 选择&#x200B;__密钥库__&#x200B;选项卡。
1. 创建或打开密钥库。 如果创建密钥库，请保护密码安全。
   + 仅当需要AuthnRequest签名/SAML断言加密时，[公钥/私钥存储才安装在此密钥存储](#install-aem-public-private-key-pair)中。
   + 如果此SAML集成支持注销，但不支持AuthnRequest签名/SAML断言，则空密钥库就足够了。
1. 选择&#x200B;__保存并关闭__。
1. 创建包含更新的&#x200B;__authentication-service__&#x200B;用户的包。

   _使用包使用以下临时解决方法：_

   1. 导航到&#x200B;__工具>部署>包__。
   1. 创建资源包
      + 包名称： `Authentication Service`
      + 版本： `1.0.0`
      + 组： `com.your.company`
   1. 编辑新的&#x200B;__身份验证服务密钥存储__&#x200B;包。
   1. 选择&#x200B;__筛选器__&#x200B;选项卡，并为根路径`/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore`添加筛选器。
      + 导航到&#x200B;__工具>安全>用户__&#x200B;并选择&#x200B;__身份验证服务__&#x200B;用户，即可找到`<AUTHENTICATION SERVICE UUID>`。 UUID是URL的最后一部分。
   1. 选择&#x200B;__完成__，然后选择&#x200B;__保存__。
   1. 为&#x200B;__Authentication Service Key Store__&#x200B;包选择&#x200B;__生成__&#x200B;按钮。
   1. 生成后，选择&#x200B;__更多__ > __复制__&#x200B;以激活AEM Publish的身份验证服务密钥存储。

## 安装AEM公钥/私钥对{#install-aem-public-private-key-pair}

_安装AEM公钥/私钥对是可选的_

AEM Publish可以配置为对AuthnRequests签名（到IDP），并加密SAML断言(到AEM)。 这是通过提供专用密钥到AEM Publish来实现的，并且它将公共密钥与IDP相匹配。

+++ 了解AuthnRequest签名流程（可选）

AuthnRequest(从AEM Publish向启动登录流程的IDP发出的请求)可由AEM Publish签名。 为此，AEM Publish使用私钥对AuthnRequest进行签名，然后IDP使用公钥验证签名。 这向IDP保证AuthnRequest是由AEM Publish发起和请求的，而不是恶意的第三方。

![SAML 2.0 - SP AuthnRequest签名](./assets/saml-2-0/sp-authnrequest-signing-diagram.png)

1. 用户向AEM Publish发出HTTP请求，从而向IDP发出SAML身份验证请求。
1. AEM Publish会生成要发送到IDP的SAML请求。
1. AEM Publish使用AEM私钥对SAML请求进行签名。
1. AEM Publish启动AuthnRequest，这是一个HTTP客户端重定向，指向包含已签名SAML请求的IDP。
1. IDP接收AuthnRequest，并使用AEM的公共密钥验证签名，从而确保AEM Publish启动了AuthnRequest。
1. 然后，AEM Publish使用IDP公共证书验证解密的SAML断言的完整性和真实性。

+++

+++ 了解SAML断言加密流程（可选）

IDP和AEM Publish之间的所有HTTP通信都应通过HTTPS，因此默认情况下是安全的。 但是，根据需要，可以在需要在HTTPS提供的保密性之外额外保密的情况下对SAML断言进行加密。 为此，IDP使用私钥加密SAML断言数据，而AEM Publish使用私钥解密SAML断言。

![SAML 2.0 - SP SAML断言加密](./assets/saml-2-0/sp-samlrequest-encryption-diagram.png)

1. 用户向IDP进行身份验证。
1. IDP生成包含用户数据的SAML声明，并使用IDP的私有证书对其进行签名。
1. 然后，IDP使用AEM公钥加密SAML断言，该密钥需要AEM私钥进行解密。
1. 加密的SAML声明通过用户的Web浏览器发送到AEM Publish。
1. AEM Publish接收SAML断言，并使用AEM私钥对其进行解密。
1. IDP提示用户进行身份验证。

+++

AuthnRequest签名和SAML断言加密都是可选的，但是它们都使用[SAML 2.0身份验证处理程序OSGi配置属性`useEncryption`](#saml-20-authenticationsaml-2-0-authentication)启用，这意味着两者都无法使用或都不使用。

![AEM身份验证服务密钥存储](./assets/saml-2-0/authentication-service-key-store.png)

1. 获取用于签署AuthnRequest的公钥、私钥（DER格式中的PKCS#8）和证书链文件（这可能是公钥），并加密SAML断言。 这些密钥通常由IT组织的安全团队提供。

   + 可使用&#x200B;__openssl__&#x200B;生成自签名密钥对：

   ```
   $ openssl req -x509 -sha256 -days 365 -newkey rsa:4096 -keyout aem-private.key -out aem-public.crt
   
   # Provide a password (keep in safe place), and other requested certificate information
   
   # Convert the keys to AEM's required format 
   $ openssl rsa -in aem-private.key -outform der -out aem-private.der
   $ openssl pkcs8 -topk8 -inform der -nocrypt -in aem-private.der -outform der -out aem-private-pkcs8.der
   ```

1. 将公钥上传到IDP。
   + 使用上述`openssl`方法，公钥是`aem-public.crt`文件。
1. 以AEM管理员身份登录AEM Author以上传私钥。
1. 导航到&#x200B;__工具>安全>信任存储区__，选择&#x200B;__身份验证服务__&#x200B;用户，然后从顶部操作栏中选择&#x200B;__属性__。
1. 导航到&#x200B;__工具>安全>用户__，选择&#x200B;__身份验证服务__&#x200B;用户，然后从顶部操作栏中选择&#x200B;__属性__。
1. 选择&#x200B;__密钥库__&#x200B;选项卡。
1. 创建或打开密钥库。 如果创建密钥库，请保护密码安全。
1. 选择&#x200B;__从DER文件添加私钥__，并将私钥和链文件添加到AEM：
   + __别名__：提供有意义的名称，通常是IDP的名称。
   + __私钥文件__：上载私钥文件（DER格式为PKCS#8）。
      + 使用上述`openssl`方法，这是`aem-private-pkcs8.der`文件
   + __选择证书链文件__：上载随附的链文件（这可能是公钥）。
      + 使用上述`openssl`方法，这是`aem-public.crt`文件
   + 选择&#x200B;__提交__
1. 新添加的证书显示在&#x200B;__从CRT文件__&#x200B;添加证书部分上方。
   + 记下&#x200B;__别名__，因为该别名在[SAML 2.0身份验证处理程序OSGi配置](#saml-20-authentication-handler-osgi-configuration)中使用
1. 选择&#x200B;__保存并关闭__。
1. 创建包含更新的&#x200B;__authentication-service__&#x200B;用户的包。

   _使用包使用以下临时解决方法：_

   1. 导航到&#x200B;__工具>部署>包__。
   1. 创建资源包
      + 包名称： `Authentication Service`
      + 版本： `1.0.0`
      + 组： `com.your.company`
   1. 编辑新的&#x200B;__身份验证服务密钥存储__&#x200B;包。
   1. 选择&#x200B;__筛选器__&#x200B;选项卡，并为根路径`/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore`添加筛选器。
      + 导航到&#x200B;__工具>安全>用户__&#x200B;并选择&#x200B;__身份验证服务__&#x200B;用户，即可找到`<AUTHENTICATION SERVICE UUID>`。 UUID是URL的最后一部分。
   1. 选择&#x200B;__完成__，然后选择&#x200B;__保存__。
   1. 为&#x200B;__Authentication Service Key Store__&#x200B;包选择&#x200B;__生成__&#x200B;按钮。
   1. 生成后，选择&#x200B;__更多__ > __复制__&#x200B;以激活AEM Publish的身份验证服务密钥存储。

## 配置SAML 2.0身份验证处理程序{#configure-saml-2-0-authentication-handler}

AEM SAML配置是通过&#x200B;__AdobeGranite SAML 2.0身份验证处理程序__ OSGi配置执行的。
该配置是OSGi工厂配置，这意味着单个AEM as a Cloud Service Publish服务可能有多个SAML配置，其涵盖存储库的离散资源树；这对于多站点AEM部署很有用。

+++ SAML 2.0身份验证处理程序OSGi配置术语表

### AdobeGranite SAML 2.0身份验证处理程序OSGi配置{#configure-saml-2-0-authentication-handler-osgi-configuration}

|                                   | OSGi属性 | 必填 | 值格式 | 默认值 | 描述 |
|-----------------------------------|-------------------------------|:--------:|:---------------------:|---------------------------|-------------|
| 路径 | `path` | ✔ | 字符串数组 | `/` | 此身份验证处理程序用于的AEM路径。 |
| IDP URL | `idpUrl` | ✔ | 字符串 |                           | 发送SAML身份验证请求的IDP URL。 |
| IDP证书别名 | `idpCertAlias` | ✔ | 字符串 |                           | 在AEM全局信任存储区中找到的IDP证书的别名 |
| IDP HTTP重定向 | `idpHttpRedirect` | ✘ | 布尔值 | `false` | 指示是否向IDP URL进行HTTP重定向而不是发送AuthnRequest。 对于IDP启动的身份验证，设置为`true`。 |
| IDP标识符 | `idpIdentifier` | ✘ | 字符串 |                           | 用于确保AEM用户和组唯一性的唯一IDP ID。 如果为空，则改用`serviceProviderEntityId`。 |
| 断言使用者服务URL | `assertionConsumerServiceURL` | ✘ | 字符串 |                           | AuthnRequest中的`AssertionConsumerServiceURL` URL属性，用于指定必须将`<Response>`消息发送到AEM的位置。 |
| SP实体ID | `serviceProviderEntityId` | ✔ | 字符串 |                           | 向IDP唯一标识AEM；通常是AEM主机名。 |
| SP加密 | `useEncryption` | ✘ | 布尔值 | `true` | 指示IDP是否加密SAML断言。 需要设置`spPrivateKeyAlias`和`keyStorePassword`。 |
| SP私钥别名 | `spPrivateKeyAlias` | ✘ | 字符串 |                           | `authentication-service`用户密钥存储中私钥的别名。 如果`useEncryption`设置为`true`，则此为必填字段。 |
| SP密钥存储密码 | `keyStorePassword` | ✘ | 字符串 |                           | “authentication-service”用户的密钥存储的密码。 如果`useEncryption`设置为`true`，则此为必填字段。 |
| 默认重定向 | `defaultRedirectUrl` | ✘ | 字符串 | `/` | 成功身份验证后的默认重定向URL。 可以相对于AEM主机（例如，`/content/wknd/us/en/html`）。 |
| 用户ID属性 | `userIDAttribute` | ✘ | 字符串 | `uid` | 包含AEM用户的用户ID的SAML断言属性的名称。 留空以使用`Subject:NameId`。 |
| 自动创建AEM用户 | `createUser` | ✘ | 布尔值 | `true` | 指示是否在成功验证时创建AEM用户。 |
| AEM用户中间路径 | `userIntermediatePath` | ✘ | 字符串 |                           | 创建AEM用户时，此值用作中间路径（例如，`/home/users/<userIntermediatePath>/jane@wknd.com`）。 需要将`createUser`设置为`true`。 |
| AEM用户属性 | `synchronizeAttributes` | ✘ | 字符串数组 |                           | 要存储在AEM用户上的SAML属性映射列表，格式为`[ "saml-attribute-name=path/relative/to/user/node" ]`（例如`[ "firstName=profile/givenName" ]`）。 查看[本机AEM属性的完整列表](#aem-user-attributes)。 |
| 将用户添加到AEM组 | `addGroupMemberships` | ✘ | 布尔值 | `true` | 指示在成功验证后是否自动将AEM用户添加到AEM用户组。 |
| AEM组成员资格属性 | `groupMembershipAttribute` | ✘ | 字符串 | `groupMembership` | SAML断言属性的名称，该属性包含应将该用户添加到的AEM用户组的列表。 需要将`addGroupMemberships`设置为`true`。 |
| 默认AEM组 | `defaultGroups` | ✘ | 字符串数组 |                           | 已验证身份的AEM用户组的列表始终添加到（例如，`[ "wknd-user" ]`）。 需要将`addGroupMemberships`设置为`true`。 |
| NameIDPolicy格式 | `nameIdFormat` | ✘ | 字符串 | `urn:oasis:names:tc:SAML:2.0:nameid-format:transient` | 在AuthnRequest消息中发送的NameIDPolicy格式参数的值。 |
| 存储SAML响应 | `storeSAMLResponse` | ✘ | 布尔值 | `false` | 指示`samlResponse`值是否存储在AEM `cq:User`节点上。 |
| 处理注销 | `handleLogout` | ✘ | 布尔值 | `false` | 指示此SAML身份验证处理程序是否处理注销请求。 需要设置`logoutUrl`。 |
| 注销URL | `logoutUrl` | ✘ | 字符串 |                           | 发送SAML注销请求的IDP URL。 如果`handleLogout`设置为`true`，则此为必填字段。 |
| 时钟容差 | `clockTolerance` | ✘ | 整数 | `60` | 验证SAML断言时，IDP和AEM (SP)时钟偏差容错。 |
| 摘要方法 | `digestMethod` | ✘ | 字符串 | `http://www.w3.org/2001/04/xmlenc#sha256` | IDP在签名SAML消息时使用的摘要算法。 |
| 签名方法 | `signatureMethod` | ✘ | 字符串 | `http://www.w3.org/2001/04/xmldsig-more#rsa-sha256` | IDP在签名SAML消息时使用的签名算法。 |
| 身份同步类型 | `identitySyncType` | ✘ | `default` 或 `idp` | `default` | 请勿更改AEM as a Cloud Service的`from`默认值。 |
| 服务排名 | `service.ranking` | ✘ | 整数 | `5002` | 同一`path`首选排名较高的配置。 |

### AEM用户属性{#aem-user-attributes}

AEM使用以下用户属性，这些属性可通过AdobeGranite SAML 2.0身份验证处理程序OSGi配置中的`synchronizeAttributes`属性填充。  任何IDP属性都可以同步到任何AEM用户属性，但是映射到AEM使用属性属性（如下所列）允许AEM自然地使用它们。

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

1. 根据项目要求更新值。 有关配置属性说明，请参阅上面的&#x200B;__SAML 2.0身份验证处理程序OSGi配置术语表__
1. 在以下情况下，建议使用OSGi环境变量和密钥：值可能与发布周期不同步时，或值在相似环境类型/服务层之间不同时。 默认值可使用如上所示的`$[env:..;default=the-default-value]"`语法进行设置。

如果SAML配置在不同环境之间不同，则可以使用特定属性定义每个环境（`config.publish.dev`、`config.publish.stage`和`config.publish.prod`）的OSGi配置。

### 使用加密

在[加密AuthnRequest和SAML断言](#encrypting-the-authnrequest-and-saml-assertion)时，需要以下属性： `useEncryption`、`spPrivateKeyAlias`和`keyStorePassword`。 `keyStorePassword`包含密码，因此不能将该值存储在OSGi配置文件中，而是使用[机密配置值](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#secret-configuration-values)插入

+++或者，更新OSGi配置以使用加密

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
+ `keyStorePassword`包含包含`authentication-service`用户密钥库密码的[OSGi密码配置变量](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#secret-configuration-values)。

+++

## 配置反向链接筛选条件

在SAML身份验证过程中，IDP向AEM Publish的`.../saml_login`端点发起客户端HTTPPOST。 如果IDP和AEM Publish存在于不同的源中，则AEM Publish的&#x200B;__反向链接筛选器__&#x200B;将通过OSGi配置配置为允许来自IDP源的HTTP POST。

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

如果`allow.hosts` （或`allow.hosts.regex`）在不同环境之间不同，则可以使用特定属性定义每个环境（`config.publish.dev`、`config.publish.stage`和`config.publish.prod`）的OSGi配置。

## 配置跨源资源共享(CORS)

在SAML身份验证过程中，IDP向AEM Publish的`.../saml_login`端点发起客户端HTTPPOST。 如果IDP和AEM Publish存在于不同的主机/域中，则必须将AEM Publish的&#x200B;__CRoss源资源共享(CORS)__&#x200B;配置为允许来自IDP主机/域的HTTP POST。

此HTTPPOST请求的`Origin`标头的值通常与AEM Publish主机不同，因此需要配置CORS。

在本地AEM SDK (`localhost:4503`)上测试SAML身份验证时，IDP可能会将`Origin`标头设置为`null`。 如果是，请将`"null"`添加到`alloworigin`列表。

1. 在`/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~saml.cfg.json`处的项目中创建一个OSGi配置文件
   + 将`/wknd-examples/`更改为您的项目名称
   + 文件名中`~`之后的标识符应唯一标识此配置，因此它可能是IDP的名称，如`...CORSPolicyImpl~okta.cfg.json`。 该值应为带连字符的字母数字。
1. 将以下JSON粘贴到`com.adobe.granite.cors.impl.CORSPolicyImpl~...cfg.json`文件中。

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

如果`alloworigin`和`allowedpaths`在不同环境之间不同，则可以使用特定属性定义每个环境（`config.publish.dev`、`config.publish.stage`和`config.publish.prod`）的OSGi配置。

## 配置AEM Dispatcher以允许SAML HTTP POST

成功对IDP进行身份验证后，IDP将编排HTTPPOST以返回到AEM注册的`/saml_login`端点（在IDP中配置）。 默认情况下，Dispatcher阻止了对`/saml_login`的此HTTPPOST，因此必须使用以下Dispatcher规则明确允许此请求：

1. 在IDE中打开`dispatcher/src/conf.dispatcher.d/filters/filters.any`。
1. 在文件底部添加一个允许规则，用于向以`/saml_login`结尾的URL发送HTTP POST。

```
...

# Allow SAML HTTP POST to ../saml_login end points
/0190 { /type "allow" /method "POST" /url "*/saml_login" }
```

如果已配置Apache Webserver上的URL重写(`dispatcher/src/conf.d/rewrites/rewrite.rules`)，请确保不会意外损坏对`.../saml_login`端点的请求。

### 如何为新环境中的SAML用户启用动态组成员资格

为了显着提升新AEM as a Cloud Service环境中的群组评估性能，建议在新环境中激活动态群组成员资格功能。
这也是激活数据同步时的必要步骤。 更多详细信息[此处](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier) 。
为此，请将以下属性添加到OSGI配置文件中：

`/apps/example/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~example.cfg.json`

使用此配置，用户和组将创建为[Oak外部用户](https://jackrabbit.apache.org/oak/docs/security/authentication/identitymanagement.html)。 在AEM中，外部用户和组具有由`[user name];[idp]`或`[group name];[idp]`组成的默认`rep:principalName`。
请注意，访问控制列表(ACL)与用户或组的PrincipalName相关联。
在现有部署中部署此配置时，如果先前的`identitySyncType`未指定或设置为`default`，则将创建新用户和组，并且必须将ACL应用于这些新用户和组。 请注意，外部组不能包含本地用户。 [Repoinit](https://sling.apache.org/documentation/bundles/repository-initialization.html)可用于为SAML外部组创建ACL，即使仅在用户执行登录时创建它们。
为避免对ACL进行此重构，已实施标准[迁移功能](#automatic-migration-to-dynamic-group-membership-for-existing-environments)。

### 成员资格如何存储在具有动态组成员资格的本地和外部组中

在本地组上，组成员存储在oak属性中： `rep:members`。 属性包含组中每个成员的uid列表。 其他详细信息可在[此处](https://jackrabbit.apache.org/oak/docs/security/user/membership.html#member-representation-in-the-repository)找到。
示例：

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

具有动态组成员资格的外部组不会在组条目中存储任何成员。
组成员资格而是存储在用户条目中。 其他文档可在[此处](https://jackrabbit.apache.org/oak/docs/security/authentication/external/dynamic.html)找到。 例如，这是组的OAK节点：

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

### 自动迁移到现有环境的动态组成员资格

启用此迁移后，将在用户身份验证期间执行，包括以下步骤：
1. 本地用户将迁移到外部用户，同时保留原始用户名。 这意味着已迁移的本地用户（现在充当外部用户）将保留其原始用户名，而不是遵循上一节中提到的命名语法。 将添加一个名为的附加属性： `rep:externalId`，其值为`[user name];[idp]`。 未修改用户`PrincipalName`。
2. 对于在SAML断言中收到的每个外部组，将创建一个外部组。 如果存在相应的本地组，则外部组将作为成员添加到本地组。
3. 用户被添加为外部组的成员。
4. 然后，该本地用户将从其所属的所有Saml本地组中删除。 Saml本地组由OAK属性标识： `rep:managedByIdp`。 当属性`syncType`未指定或设置为`default`时，此属性由Saml身份验证处理程序设置。

例如，如果在迁移`user1`之前是本地用户并且是本地组`group1`的成员，则迁移后将发生以下更改：
`user1`成为外部用户。 属性`rep:externalId`已添加到其配置文件。
`user1`成为外部组的成员： `group1;idp`
`user1`不再是本地组的直接成员： `group1`
`group1;idp`是本地组的成员： `group1`。
然后`user1`通过继承成为本地组`group1`的成员

外部组的组成员资格存储在属性`rep:authorizableId`的用户配置文件中

### 如何配置自动迁移到动态组成员资格

1. 在SAML OSGI配置文件`com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json`中启用属性`"identitySyncType": "idp_dynamic_simplified_id"`：
2. 使用属性配置PID： `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration~...`的新OSGI服务：

```
{
  "idpIdentifier": "<vaule of identitySyncType of saml configuration to be migrated>"
}
```

## 部署SAML配置

OSGi配置必须提交到Git并使用Cloud Manager部署到AEM as a Cloud Service。

```
$ git remote -v            
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (fetch)
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (push)
$ git add .
$ git commit -m "SAML 2.0 configurations"
$ git push adobe saml-auth:develop
```

使用全栈部署管道部署目标Cloud Manager Git分支（在此示例中为`develop`）。

## 调用SAML身份验证

可以通过创建巧尽心思构建的链接或按钮，从AEM Site网页调用SAML身份验证流程。 下面描述的参数可以根据需要进行编程设置，因此，例如，登录按钮可以根据按钮的上下文将`saml_request_path`（成功进行SAML身份验证时用户所在位置）设置为不同的AEM页面。

### GET请求

可以通过创建以下格式的HTTPGET请求来调用SAML身份验证：

`HTTP GET /system/sling/login`

并提供查询参数：

| 查询参数名称 | 查询参数值 |
|----------------------|-----------------------|
| `resource` | 任何作为SAML身份验证处理程序侦听的JCR路径或子路径，如[AdobeGranite SAML 2.0身份验证处理程序OSGi配置的](#configure-saml-2-0-authentication-handler) `path`属性中所定义。 |
| `saml_request_path` | 成功SAML身份验证后用户应采用的URL路径。 |

例如，此HTML链接将触发SAML登录流程，成功后将用户转到`/content/wknd/us/en/protected/page.html`。 可以根据需要以编程方式设置这些查询参数。

```html
<a href="/system/sling/login?resource=/content/wknd&saml_request_path=/content/wknd/us/en/protected/page.html">
    Log in using SAML
</a>
```

## POST请求

可以通过创建以下格式的HTTPPOST请求来调用SAML身份验证：

`HTTP POST /system/sling/login`

并提供表单数据：

| 表单数据名称 | 表单数据值 |
|----------------------|-----------------------|
| `resource` | 任何作为SAML身份验证处理程序侦听的JCR路径或子路径，如[AdobeGranite SAML 2.0身份验证处理程序OSGi配置的](#configure-saml-2-0-authentication-handler) `path`属性中所定义。 |
| `saml_request_path` | 成功SAML身份验证后用户应采用的URL路径。 |


例如，此“HTML”按钮将使用HTTPPOST触发SAML登录流程，成功后，将用户带到`/content/wknd/us/en/protected/page.html`。 可以根据需要以编程方式设置这些表单数据参数。

```html
<form action="/system/sling/login" method="POST">
    <input type="hidden" name="resource" value="/content/wknd">
    <input type="hidden" name="saml_request_path" value="/content/wknd/us/en/protected/page.html">
    <input type="submit" value="Log in using SAML">
</form>
```

### Dispatcher配置

HTTPGET和POST方法都需要客户端访问AEM的`/system/sling/login`端点，因此必须允许通过AEM Dispatcher访问它们。

根据是否使用了GET或POST，允许使用必要的URL模式

```
# Allow GET-based SAML authentication invocation
/0191 { /type "allow" /method "GET" /url "/system/sling/login" /query "*" }

# Allow POST-based SAML authentication invocation
/0192 { /type "allow" /method "POST" /url "/system/sling/login" }
```
