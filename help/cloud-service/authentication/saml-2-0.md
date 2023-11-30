---
title: AEMas a Cloud Service上的SAML 2.0
description: 了解如何在AEMas a Cloud Service发布服务上配置SAML 2.0身份验证。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9351
thumbnail: 343040.jpeg
last-substantial-update: 2022-10-17T00:00:00Z
exl-id: 461dcdda-8797-4a37-a0c7-efa7b3f1e23e
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '3123'
ht-degree: 2%

---

# SAML 2.0身份验证{#saml-2-0-authentication}

了解如何为您选择的SAML 2.0兼容IDP设置和验证最终用户(而非AEM作者)。

## AEMas a Cloud Service的SAML是什么？

SAML 2.0与AEM Publish（或Preview）集成，允许基于AEM的Web体验的最终用户向非AdobeIDP（身份提供程序）进行身份验证，并以已命名授权用户的身份访问AEM。

|                       | AEM Author | AEM 发布 |
|-----------------------|:----------:|:-----------:|
| SAML 2.0支持 | ✘ | ✔ |

+++ 了解使用AEM的SAML 2.0流程

AEM Publish SAML集成的典型流程如下所示：

1. 用户向AEM Publish发出请求，指示需要身份验证。
   + 用户请求受CUG/ACL保护的资源。
   + 用户请求受身份验证要求约束的资源。
   + 用户关注指向AEM登录端点的链接(即 `/system/sling/login`)来显式请求登录操作。
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
1. AEM发布设置AEM `login-token` HTTP响应上的Cookie，用于向AEM Publish验证后续请求。
1. AEM发布将用户重定向到AEM发布上的URL，如所指定 `saml_request_path` Cookie。

+++

## 配置演练

>[!VIDEO](https://video.tv.adobe.com/v/343040?quality=12&learn=on)

此视频介绍如何设置SAML 2.0与AEMas a Cloud Service发布服务的集成，以及使用Okta作为IDP。

## 前提条件

设置SAML 2.0身份验证时需要满足以下条件：

+ 部署管理员对Cloud Manager的访问权限
+ AEM管理员对AEMas a Cloud Service环境的访问权限
+ 对IDP的管理员访问权限
+ （可选）访问用于加密SAML有效负载的公钥/私钥对

仅支持SAML 2.0向AEM发布或预览验证用户。 要使用和IDP管理AEM Author身份验证， [将IDP与Adobe IMS集成](https://helpx.adobe.com/cn/enterprise/using/set-up-identity.html).


## 在AEM上安装IDP公共证书

IDP的公共证书将添加到AEM全局信任存储区，并用于验证IDP发送的SAML断言是否有效。

+++SAML断言签名流程

![SAML 2.0 - IDP SAML断言签名](./assets/saml-2-0/idp-signing-diagram.png)

1. 用户向IDP进行身份验证。
1. IDP生成包含用户数据的SAML断言。
1. IDP使用IDP的私有证书签署SAML声明。
1. IDP向AEM发布的SAML端点发起客户端HTTPPOST(`.../saml_login`)，其中包括带符号的SAML断言。
1. AEM Publish接收包含已签名SAML断言的HTTPPOST，可以使用IDP公共证书验证签名。

+++

![将IDP公共证书添加到全局信任存储区](./assets/saml-2-0/global-trust-store.png)

1. 获取 __公共证书__ 来自IDP的文件。 该证书允许AEM验证IDP向AEM提供的SAML声明。

   证书为PEM格式，应类似于：

   ```
   -----BEGIN CERTIFICATE-----
   MIIC4jCBAcoCCQC33wnybT5QZDANBgkqhkiG9w0BAQsFADAyMQswCQYDVQQGEwJV
   ...
   m0eo2USlSRTVl7QHRTuiuSThHpLKQQ==
   -----END CERTIFICATE-----
   ```

1. 以AEM管理员身份登录AEM Author。
1. 导航到 __“工具”>“安全”>“信任存储”__.
1. 创建或打开全局信任存储区。 如果创建全局信任存储区，请将密码保存在安全的地方。
1. 展开 __从CER文件添加证书__.
1. 选择 __选择证书文件__，并上传IDP提供的证书文件。
1. 离开 __将证书映射到用户__ 空白。
1. 选择&#x200B;__提交__。
1. 新添加的证书将显示在 __从CRT文件添加证书__ 部分。
1. 记下 __别名__，因为该值用于 [SAML 2.0身份验证处理程序OSGi配置](#saml-2-0-authentication-handler-osgi-configuration).
1. 选择 __保存并关闭__.

全局信任存储区在AEM Author上使用IDP的公共证书进行配置，但由于SAML仅在AEM Publish上使用，因此必须将全局信任存储区复制到AEM Publish，才能在其中访问IDP公共证书。

![将全局信任存储区复制到AEM Publish](./assets/saml-2-0/global-trust-store-replicate.png)

1. 导航到&#x200B;__工具 > 部署 > 包__。
1. 创建资源包
   + 包名称： `Global Trust Store`
   + 版本: `1.0.0`
   + 组: `com.your.company`
1. 编辑新的 __全局信任存储区__ 包。
1. 选择 __过滤器__ 选项卡，并为根路径添加过滤器 `/etc/truststore`.
1. 选择 __完成__ 然后 __保存__.
1. 选择 __生成__ 按钮 __全局信任存储区__ 包。
1. 构建后，选择 __更多__ > __复制__ 激活全局信任存储区节点(`/etc/truststore`)到AEM Publish。

## 创建认证服务密钥库{#authentication-service-keystore}

_在以下情况下，需要为身份验证服务创建密钥库： [SAML 2.0身份验证处理程序OSGi配置属性 `handleLogout` 设置为 `true`](#saml-20-authenticationsaml-2-0-authentication) 或 [AuthnRequest签名/SAML断言加密](#install-aem-public-private-key-pair) 为必填项_

1. 以AEM管理员身份登录AEM Author以上传私钥。
1. 导航到 __“工具”>“安全”>“用户”__，并选择 __authentication-service__ 用户，并选择 __属性__ 从顶部操作栏中。
1. 选择 __密钥库__ 选项卡。
1. 创建或打开密钥库。 如果创建密钥库，请保护密码安全。
   + A [public/private keystore已安装到此keystore](#install-aem-public-private-key-pair) 仅当需要AuthnRequest签名/SAML断言加密时。
   + 如果此SAML集成支持注销，但不支持AuthnRequest签名/SAML断言，则空密钥库就足够了。
1. 选择 __保存并关闭__.
1. 创建包含更新的包 __authentication-service__ 用户。

   _使用包使用以下临时解决方法：_

   1. 导航到&#x200B;__工具 > 部署 > 包__。
   1. 创建资源包
      + 包名称： `Authentication Service`
      + 版本: `1.0.0`
      + 组: `com.your.company`
   1. 编辑新的 __身份验证服务密钥存储__ 包。
   1. 选择 __过滤器__ 选项卡，并为根路径添加过滤器 `/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore`.
      + 此 `<AUTHENTICATION SERVICE UUID>` 导航到 __“工具”>“安全”>“用户”__，并选择 __authentication-service__ 用户。 UUID是URL的最后一部分。
   1. 选择 __完成__ 然后 __保存__.
   1. 选择 __生成__ 按钮 __身份验证服务密钥存储__ 包。
   1. 构建后，选择 __更多__ > __复制__ 以激活AEM Publish的身份验证服务密钥存储。

## 安装AEM公钥/私钥对{#install-aem-public-private-key-pair}

_安装AEM公钥/私钥对是可选的_

AEM Publish可以配置为对AuthnRequests（到IDP）进行签名，并对SAML断言(到AEM)进行加密。 这是通过提供专用密钥到AEM Publish来实现的，并且它将公共密钥与IDP相匹配。

+++ 了解AuthnRequest签名流程（可选）

AuthnRequest(从AEM Publish向IDP发出的启动登录过程的请求)可由AEM Publish签名。 为此，AEM Publish使用私钥对AuthnRequest签名，然后IDP使用公钥验证签名。 这向IDP保证AuthnRequest是由AEM Publish发起和请求的，而不是恶意的第三方。

![SAML 2.0 - SP AuthnRequest签名](./assets/saml-2-0/sp-authnrequest-signing-diagram.png)

1. 用户向AEM Publish发出HTTP请求，从而向IDP发出SAML身份验证请求。
1. AEM Publish生成要发送到IDP的SAML请求。
1. AEM发布使用AEM私钥对SAML请求进行签名。
1. AEM Publish会启动AuthnRequest，这是一个HTTP客户端重定向，指向包含已签名SAML请求的IDP。
1. IDP接收AuthnRequest，并使用AEM公钥验证签名，从而确保AEM Publish启动了AuthnRequest。
1. 然后，AEM Publish使用IDP公共证书验证解密的SAML声明的完整性和真实性。

+++

+++ 了解SAML断言加密流程（可选）

IDP和AEM Publish之间的所有HTTP通信都应通过HTTPS，因此默认情况下是安全的。 但是，根据需要，可以在需要在HTTPS提供的保密性之外额外保密的情况下对SAML断言进行加密。 为此，IDP使用私钥加密SAML断言数据，而AEM Publish使用私钥解密SAML断言。

![SAML 2.0 - SP SAML断言加密](./assets/saml-2-0/sp-samlrequest-encryption-diagram.png)

1. 用户向IDP进行身份验证。
1. IDP生成包含用户数据的SAML声明，并使用IDP的私有证书对其进行签名。
1. 然后，IDP使用AEM公钥加密SAML断言，这需要AEM私钥进行解密。
1. 加密的SAML声明通过用户的Web浏览器发送到AEM Publish。
1. AEM Publish接收SAML断言，并使用AEM私钥对其进行解密。
1. IDP提示用户进行身份验证。

+++

AuthnRequest签名和SAML断言加密都是可选的，但使用 [SAML 2.0身份验证处理程序OSGi配置属性 `useEncryption`](#saml-20-authenticationsaml-2-0-authentication)，这意味着两者都不能使用。

![AEM authentication-service密钥存储](./assets/saml-2-0/authentication-service-key-store.png)

1. 获取用于签署AuthnRequest的公钥、私钥（DER格式中的PKCS#8）和证书链文件（这可能是公钥），并加密SAML断言。 这些密钥通常由IT组织的安全团队提供。

   + 可以使用生成自签名密钥对 __openssl__：

   ```
   $ openssl req -x509 -sha256 -days 365 -newkey rsa:4096 -keyout aem-private.key -out aem-public.crt
   
   # Provide a password (keep in safe place), and other requested certificate information
   
   # Convert the keys to AEM's required format 
   $ openssl rsa -in aem-private.key -outform der -out aem-private.der
   $ openssl pkcs8 -topk8 -inform der -nocrypt -in aem-private.der -outform der -out aem-private-pkcs8.der
   ```

1. 将公钥上传到IDP。
   + 使用 `openssl` 方法上，公钥是 `aem-public.crt` 文件。
1. 以AEM管理员身份登录AEM Author以上传私钥。
1. 导航到 __“工具”>“安全”>“信任存储”__，并选择 __authentication-service__ 用户，并选择 __属性__ 从顶部操作栏中。
1. 导航到 __“工具”>“安全”>“用户”__，并选择 __authentication-service__ 用户，并选择 __属性__ 从顶部操作栏中。
1. 选择 __密钥库__ 选项卡。
1. 创建或打开密钥库。 如果创建密钥库，请保护密码安全。
1. 选择 __从DER文件添加私钥__，并将私钥和链文件添加到AEM：
   + __别名__：提供有意义的名称，通常是IDP的名称。
   + __私钥文件__：上传私钥文件（DER格式的PKCS#8）。
      + 使用 `openssl` 方法上，这是 `aem-private-pkcs8.der` 文件
   + __选择证书链文件__：上传随附的链文件（可能是公钥）。
      + 使用 `openssl` 方法上，这是 `aem-public.crt` 文件
   + 选择 __提交__
1. 新添加的证书将显示在 __从CRT文件添加证书__ 部分。
   + 记下 __别名__ 因为此函数用于 [SAML 2.0身份验证处理程序OSGi配置](#saml-20-authentication-handler-osgi-configuration)
1. 选择 __保存并关闭__.
1. 创建包含更新的包 __authentication-service__ 用户。

   _使用包使用以下临时解决方法：_

   1. 导航到&#x200B;__工具 > 部署 > 包__。
   1. 创建资源包
      + 包名称： `Authentication Service`
      + 版本: `1.0.0`
      + 组: `com.your.company`
   1. 编辑新的 __身份验证服务密钥存储__ 包。
   1. 选择 __过滤器__ 选项卡，并为根路径添加过滤器 `/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore`.
      + 此 `<AUTHENTICATION SERVICE UUID>` 导航到 __“工具”>“安全”>“用户”__，并选择 __authentication-service__ 用户。 UUID是URL的最后一部分。
   1. 选择 __完成__ 然后 __保存__.
   1. 选择 __生成__ 按钮 __身份验证服务密钥存储__ 包。
   1. 构建后，选择 __更多__ > __复制__ 以激活AEM Publish的身份验证服务密钥存储。

## 配置SAML 2.0身份验证处理程序{#configure-saml-2-0-authentication-handler}

AEM SAML配置是通过 __AdobeGranite SAML 2.0身份验证处理程序__ osgi配置。
该配置是OSGi工厂配置，这意味着单个AEMas a Cloud Service发布服务可能具有覆盖存储库离散资源树的多个SAML配置；这对于多站点AEM部署很有用。

+++ SAML 2.0身份验证处理程序OSGi配置术语表

### AdobeGranite SAML 2.0身份验证处理程序OSGi配置{#configure-saml-2-0-authentication-handler-osgi-configuration}

|                                   | OSGi属性 | 必填 | 值格式 | 默认值 | 描述 |
|-----------------------------------|-------------------------------|:--------:|:---------------------:|---------------------------|-------------|
| 路径 | `path` | ✔ | 字符串数组 | `/` | 此身份验证处理程序用于的AEM路径。 |
| IDP URL | `idpUrl` | ✔ | 字符串 |                           | 发送SAML身份验证请求的IDP URL。 |
| IDP证书别名 | `idpCertAlias` | ✔ | 字符串 |                           | 在AEM全局信任存储区中找到的IDP证书的别名 |
| IDP HTTP重定向 | `idpHttpRedirect` | ✘ | 布尔值 | `false` | 指示是否向IDP URL进行HTTP重定向而不是发送AuthnRequest。 设置为 `true` IDP启动的身份验证。 |
| IDP标识符 | `idpIdentifier` | ✘ | 字符串 |                           | 用于确保AEM用户和组唯一性的唯一IDP ID。 如果为空， `serviceProviderEntityId` 将改用。 |
| 断言使用者服务URL | `assertionConsumerServiceURL` | ✘ | 字符串 |                           | 此 `AssertionConsumerServiceURL` AuthnRequest中的URL属性，指定 `<Response>` 消息必须发送到AEM。 |
| SP实体ID | `serviceProviderEntityId` | ✔ | 字符串 |                           | 向IDP唯一标识AEM；通常是AEM主机名。 |
| SP加密 | `useEncryption` | ✘ | 布尔值 | `true` | 指示IDP是否加密SAML断言。 需要 `spPrivateKeyAlias` 和 `keyStorePassword` 将设置。 |
| SP私钥别名 | `spPrivateKeyAlias` | ✘ | 字符串 |                           | 中私钥的别名 `authentication-service` 用户的密钥存储。 在以下情况下需要 `useEncryption` 设置为 `true`. |
| SP密钥存储密码 | `keyStorePassword` | ✘ | 字符串 |                           | “authentication-service”用户的密钥存储的密码。 在以下情况下需要 `useEncryption` 设置为 `true`. |
| 默认重定向 | `defaultRedirectUrl` | ✘ | 字符串 | `/` | 成功身份验证后的默认重定向URL。 可以相对于AEM主机(例如， `/content/wknd/us/en/html`)。 |
| 用户ID属性 | `userIDAttribute` | ✘ | 字符串 | `uid` | 包含AEM用户的用户ID的SAML断言属性的名称。 留空以使用 `Subject:NameId`. |
| 自动创建AEM用户 | `createUser` | ✘ | 布尔值 | `true` | 指示是否在成功验证时创建AEM用户。 |
| AEM用户中间路径 | `userIntermediatePath` | ✘ | 字符串 |                           | 创建AEM用户时，此值用作中间路径(例如， `/home/users/<userIntermediatePath>/jane@wknd.com`)。 需要 `createUser` 将设置为 `true`. |
| AEM用户属性 | `synchronizeAttributes` | ✘ | 字符串数组 |                           | 要存储在AEM用户上的SAML属性映射列表，格式为 `[ "saml-attribute-name=path/relative/to/user/node" ]` (例如， `[ "firstName=profile/givenName" ]`)。 请参阅 [本机AEM属性的完整列表](#aem-user-attributes). |
| 将用户添加到AEM组 | `addGroupMemberships` | ✘ | 布尔值 | `true` | 指示在成功验证后是否自动将AEM用户添加到AEM用户组。 |
| AEM组成员资格属性 | `groupMembershipAttribute` | ✘ | 字符串 | `groupMembership` | SAML断言属性的名称，该属性包含应将该用户添加到的AEM用户组的列表。 需要 `addGroupMemberships` 将设置为 `true`. |
| 默认AEM组 | `defaultGroups` | ✘ | 字符串数组 |                           | 经过身份验证的用户的AEM用户组列表始终添加到(例如， `[ "wknd-user" ]`)。 需要 `addGroupMemberships` 将设置为 `true`. |
| NameIDPolicy格式 | `nameIdFormat` | ✘ | 字符串 | `urn:oasis:names:tc:SAML:2.0:nameid-format:transient` | 在AuthnRequest消息中发送的NameIDPolicy格式参数的值。 |
| 存储SAML响应 | `storeSAMLResponse` | ✘ | 布尔值 | `false` | 指示是否 `samlResponse` 值存储在AEM上 `cq:User` 节点。 |
| 处理注销 | `handleLogout` | ✘ | 布尔值 | `false` | 指示此SAML身份验证处理程序是否处理注销请求。 需要 `logoutUrl` 将设置。 |
| 注销URL | `logoutUrl` | ✘ | 字符串 |                           | 发送SAML注销请求的IDP URL。 在以下情况下需要 `handleLogout` 设置为 `true`. |
| 时钟容差 | `clockTolerance` | ✘ | 整数 | `60` | 验证SAML断言时，IDP和AEM (SP)时钟偏差容错。 |
| 摘要方法 | `digestMethod` | ✘ | 字符串 | `http://www.w3.org/2001/04/xmlenc#sha256` | IDP在签名SAML消息时使用的摘要算法。 |
| 签名方法 | `signatureMethod` | ✘ | 字符串 | `http://www.w3.org/2001/04/xmldsig-more#rsa-sha256` | IDP在签名SAML消息时使用的签名算法。 |
| 身份同步类型 | `identitySyncType` | ✘ | `default` 或 `idp` | `default` | 请勿更改 `from` AEMas a Cloud Service的默认值。 |
| 服务排名 | `service.ranking` | ✘ | 整数 | `5002` | 对于相同的配置，最好使用排名较高的配置 `path`. |

### AEM用户属性{#aem-user-attributes}

AEM使用以下用户属性，这些属性可通过 `synchronizeAttributes` 属性，此属性位于AdobeGranite SAML 2.0身份验证处理程序OSGi配置中。  任何IDP属性都可以同步到任何AEM用户属性，但是映射到AEM使用属性属性（如下所列）允许AEM自然地使用它们。

| 用户属性 | 相对属性路径自 `rep:User` 节点 |
|--------------------------------|--------------------------|
| 标题(例如， `Mrs`) | `profile/title` |
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

1. 在项目中创建一个OSGi配置文件，位于 `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` 并在IDE中打开。
   + 更改 `/wknd-examples/` 敬您的 `/<project name>/`
   + 后面的标识符 `~` 文件名中应唯一标识此配置，因此它可能是IDP的名称，例如 `...~okta.cfg.json`. 该值应为带连字符的字母数字。
1. 将以下JSON粘贴到 `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json` 文件，并更新 `wknd` 根据需要引用。

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

1. 根据项目要求更新值。 请参阅 __SAML 2.0身份验证处理程序OSGi配置术语表__ 以上有关配置属性的说明
1. 在以下情况下，建议使用OSGi环境变量和密钥：值可能与发布周期不同步时，或值在相似环境类型/服务层之间不同时。 可以使用以下代码设置默认值： `$[env:..;default=the-default-value]"` 语法，如上所示。

每个环境的OSGi配置(`config.publish.dev`， `config.publish.stage`、和 `config.publish.prod`如果SAML配置在不同环境之间有所不同，则可以使用特定属性定义。

### 使用加密

时间 [加密AuthnRequest和SAML断言](#encrypting-the-authnrequest-and-saml-assertion)，则需要以下属性： `useEncryption`， `spPrivateKeyAlias`、和 `keyStorePassword`. 此 `keyStorePassword` 包含密码，因此值不能存储在OSGi配置文件中，而是使用插入 [密码配置值](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#secret-configuration-values)

+++或者，更新OSGi配置以使用加密

1. 打开 `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` 在IDE中。
1. 添加三个属性 `useEncryption`， `spPrivateKeyAlias`、和 `keyStorePassword` 如下所示。

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

+ `useEncryption` 设置为 `true`
+ `spPrivateKeyAlias` 包含SAML集成使用的私钥的密钥库条目别名。
+ `keyStorePassword` 包含 [OSGi密码配置变量](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#secret-configuration-values) 包含 `authentication-service` 用户密钥库的密码。

+++

## 配置反向链接筛选条件

POST在SAML身份验证过程中，IDP向AEM Publish的 `.../saml_login` 终点。 如果IDP和AEM Publish存在于不同的来源，则AEM Publish的 __反向链接筛选条件__ 通过OSGi配置配置为允许来自IDP源的HTTP POST。

1. 在项目中创建（或编辑）OSGi配置文件： `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/org.apache.sling.security.impl.ReferrerFilter.cfg.json`.
   + 更改 `/wknd-examples/` 敬您的 `/<project name>/`
1. 确保 `allow.empty` 值设置为 `true`， `allow.hosts` (或者，如果您愿意， `allow.hosts.regexp`)包含IDP的来源，以及 `filter.methods` 包含 `POST`. OSGi配置应类似于：

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

每个环境的OSGi配置(`config.publish.dev`， `config.publish.stage`、和 `config.publish.prod`)可以使用特定属性定义，如果 `allow.hosts` (或 `allow.hosts.regex`)因环境而异。

## 配置跨源资源共享(CORS)

POST在SAML身份验证过程中，IDP向AEM Publish的 `.../saml_login` 终点。 如果IDP和AEM Publish存在于不同的主机/域中，则AEM Publish的 __CRoss源资源共享(CORS)__ 必须配置为允许来自IDP主机/域的HTTP POST。

此HTTPPOST请求的 `Origin` 标头的值通常与AEM Publish主机的值不同，因此需要进行CORS配置。

在本地AEM SDK上测试SAML身份验证时(`localhost:4503`)，则IDP可以设置 `Origin` 标题至 `null`. 如果是这样的话，请添加 `"null"` 到 `alloworigin` 列表。

1. 在项目中创建一个OSGi配置文件，位于 `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~saml.cfg.json`
   + 更改 `/wknd-examples/` 到您的项目名称
   + 后面的标识符 `~` 文件名中应唯一标识此配置，因此它可能是IDP的名称，例如 `...CORSPolicyImpl~okta.cfg.json`. 该值应为带连字符的字母数字。
1. 将以下JSON粘贴到 `com.adobe.granite.cors.impl.CORSPolicyImpl~...cfg.json` 文件。

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

每个环境的OSGi配置(`config.publish.dev`， `config.publish.stage`、和 `config.publish.prod`)可以使用特定属性定义，如果 `alloworigin` 和 `allowedpaths` 因环境而异。

## 配置AEM Dispatcher以允许SAML HTTP POST

成功向IDP进行身份验证后，IDP将编排HTTPPOST以使其返回已注册的AEM `/saml_login` 端点（在IDP中配置）。 此HTTPPOST用于 `/saml_login` 默认情况下，Dispatcher上会阻止该活动，因此必须使用以下Dispatcher规则明确允许该活动：

1. 打开 `dispatcher/src/conf.dispatcher.d/filters/filters.any` 在IDE中。
1. 在文件底部添加一个允许规则，用于允许对以结尾的URL执行HTTP POST `/saml_login`.

```
...

# Allow SAML HTTP POST to ../saml_login end points
/0190 { /type "allow" /method "POST" /url "*/saml_login" }
```

如果已配置Apache Webserver上的URL重写(`dispatcher/src/conf.d/rewrites/rewrite.rules`)，确保向 `.../saml_login` 不会意外损坏终结点。

## 启用数据同步并封装令牌

一旦SAML身份验证流程在AEM Publish中创建用户，AEM用户节点就可以跨AEM Publish服务层进行身份验证。
这需要 [数据同步](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html#data-synchronization) 和 [封装令牌](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html#sticky-sessions-and-encapsulated-tokens) 将由AEM Publish服务上的Adobe支持启用。

向Adobe客户支持发送请求(通过 [AdminConsole](https://adminconsole.adobe.com) >支持)请求：

> 在程序X和环境Y的AEM Publish服务上启用了数据同步和封装令牌。

## 部署SAML配置

OSGi配置必须提交到Git并使用Cloud Manager部署到AEMas a Cloud Service。

```
$ git remote -v            
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (fetch)
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (push)
$ git add .
$ git commit -m "SAML 2.0 configurations"
$ git push adobe saml-auth:develop
```

部署目标Cloud Manager Git分支(在此示例中， `develop`)，使用全栈栈部署管道。
