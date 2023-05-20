---
title: AEMas a Cloud Service上的SAML 2.0
description: 瞭解如何在AEMas a Cloud Service發佈服務上設定SAML 2.0驗證。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9351
thumbnail: 343040.jpeg
last-substantial-update: 2022-10-17T00:00:00Z
exl-id: 461dcdda-8797-4a37-a0c7-efa7b3f1e23e
source-git-commit: f6a9e7b32d876a8cd5ce7bf6a2e13aeb5faaf35b
workflow-type: tm+mt
source-wordcount: '3123'
ht-degree: 2%

---

# SAML 2.0驗證{#saml-2-0-authentication}

瞭解如何為您選擇的SAML 2.0相容的IDP設定及驗證一般使用者(而非AEM作者)。

## AEM適用的SAMLas a Cloud Service為何？

SAML 2.0與AEM Publish （或Preview）整合，可讓AEM型網頁體驗的一般使用者向非AdobeIDP （身分提供者）進行驗證，並以已命名的授權使用者身分存取AEM。

|  | AEM Author | AEM 发布 |
|-----------------------|:----------:|:-----------:|
| SAML 2.0支援 | ✘ | ✔ |

+++ 瞭解使用AEM的SAML 2.0流程

AEM Publish SAML整合的典型流程如下：

1. 使用者向AEM Publish提出要求，表示需要驗證。
   + 使用者請求受CUG/ACL保護的資源。
   + 使用者請求受驗證需求約束的資源。
   + 使用者依循指向AEM登入端點的連結(即 `/system/sling/login`)以明確要求登入動作。
1. AEM會向IDP發出AuthnRequest，要求IDP啟動驗證程式。
1. 使用者向IDP進行驗證。
   + IDP會提示使用者輸入認證。
   + 使用者已經通過IDP驗證，無需提供進一步的認證。
1. IDP會產生包含使用者資料的SAML宣告，並使用IDP的私人憑證加以簽署。
1. IDP會透過使用者的網頁瀏覽器，透過HTTPPOST將SAML宣告傳送至AEM Publish。
1. AEM Publish會收到SAML判斷提示，並使用IDP公開憑證來驗證SAML判斷提示的完整性和真實性。
1. AEM Publish會根據SAML 2.0 OSGi設定和SAML宣告的內容來管理AEM使用者記錄。
   + 建立使用者
   + 同步使用者屬性
   + 更新AEM使用者群組成員資格
1. AEM發佈設定AEM `login-token` HTTP回應上的Cookie，用於向AEM Publish驗證後續請求。
1. AEM發佈會將使用者重新導向至AEM發佈上的URL，如所指定 `saml_request_path` Cookie。

+++

## 設定逐步說明

>[!VIDEO](https://video.tv.adobe.com/v/343040?quality=12&learn=on)

此影片逐步解說如何設定SAML 2.0與AEMas a Cloud Service發佈服務的整合，以及使用Okta作為IDP。

## 前提条件

設定SAML 2.0驗證時需要下列專案：

+ 部署管理員對Cloud Manager的存取權
+ AEM管理員對AEMas a Cloud Service環境的存取權
+ IDP的管理員存取權
+ 選擇性地存取用來加密SAML裝載的公用/私用金鑰組

僅支援SAML 2.0向AEM Publish或Preview驗證使用者。 若要使用和IDP管理AEM作者驗證， [將IDP與Adobe IMS整合](https://helpx.adobe.com/cn/enterprise/using/set-up-identity.html).


## 在AEM上安裝IDP公開憑證

IDP的公開憑證會新增至AEM全域信任存放區，並用來驗證IDP傳送的SAML宣告是否有效。

+++SAML宣告簽署流程

![SAML 2.0 - IDP SAML判斷提示簽署](./assets/saml-2-0/idp-signing-diagram.png)

1. 使用者向IDP進行驗證。
1. IDP會產生包含使用者資料的SAML判斷提示。
1. IDP會使用IDP的私人憑證來簽署SAML宣告。
1. IDP向AEM發佈的SAML端點起始使用者端HTTPPOST(`.../saml_login`)，其中包含簽署的SAML判斷提示。
1. AEM Publish會收到包含已簽署SAML宣告的HTTPPOST，可以使用IDP公開憑證來驗證簽名。

+++

![將IDP公開憑證新增至全域信任存放區](./assets/saml-2-0/global-trust-store.png)

1. 取得 __公開憑證__ IDP中的檔案。 此憑證可讓AEM驗證IDP提供給AEM的SAML判斷提示。

   憑證為PEM格式，應類似於：

   ```
   -----BEGIN CERTIFICATE-----
   MIIC4jCBAcoCCQC33wnybT5QZDANBgkqhkiG9w0BAQsFADAyMQswCQYDVQQGEwJV
   ...
   m0eo2USlSRTVl7QHRTuiuSThHpLKQQ==
   -----END CERTIFICATE-----
   ```

1. 以AEM管理員身分登入AEM作者。
1. 導覽至 __「工具>安全性>信任存放區」__.
1. 建立或開啟全域信任存放區。 如果建立全域信任存放區，請將密碼存放於安全的位置。
1. 展開 __從CER檔案新增憑證__.
1. 選取 __選取憑證檔案__，並上傳IDP提供的憑證檔案。
1. 離開 __將憑證對應到使用者__ 空白。
1. 選取 __提交__.
1. 新新增的憑證會顯示在 __從CRT檔案新增憑證__ 區段。
1. 記下 __別名__，此值用於 [SAML 2.0驗證處理常式OSGi設定](#saml-2-0-authentication-handler-osgi-configuration).
1. 選取 __儲存並關閉__.

全域信任存放區已在AEM Author上使用IDP的公開憑證設定，但由於SAML僅用於AEM Publish，因此必須將全域信任存放區復寫至AEM Publish，才能在IDP公開憑證中存取。

![將全域信任存放區復寫至AEM發佈](./assets/saml-2-0/global-trust-store-replicate.png)

1. 导航到&#x200B;__工具 > 部署 > 包__。
1. 建立套件
   + 封裝名稱： `Global Trust Store`
   + 版本: `1.0.0`
   + 组: `com.your.company`
1. 編輯新的 __全域信任存放區__ 封裝。
1. 選取 __篩選器__ 標籤，並為根路徑新增篩選器 `/etc/truststore`.
1. 選取 __完成__ 然後 __儲存__.
1. 選取 __建置__ 「 」的按鈕 __全域信任存放區__ 封裝。
1. 建置後，選取 __更多__ > __復寫__ 啟動全域信任存放區節點(`/etc/truststore`)到AEM Publish。

## 建立驗證服務金鑰存放區{#authentication-service-keystore}

_當下列情況時，需要建立驗證服務的金鑰存放區： [SAML 2.0驗證處理常式OSGi設定屬性 `handleLogout` 設為 `true`](#saml-20-authenticationsaml-2-0-authentication) 或 [AuthnRequest簽署/SAML宣告加密](#install-aem-public-private-key-pair) 為必填_

1. 以AEM管理員身分登入AEM作者以上傳私密金鑰。
1. 導覽至 __「工具>安全性>使用者」__，並選取 __authentication-service__ 使用者，並選取 __屬性__ 從頂端動作列取得。
1. 選取 __金鑰存放區__ 標籤。
1. 建立或開啟金鑰存放區。 如果建立金鑰存放區，請妥善保管密碼。
   + A [公開/私密金鑰存放區已安裝至此金鑰存放區](#install-aem-public-private-key-pair) 僅限需要AuthnRequest簽署/SAML宣告加密時。
   + 如果此SAML整合支援登出，但不支援AuthnRequest簽署/SAML判斷提示，則空的金鑰儲存區就足夠了。
1. 選取 __儲存並關閉__.
1. 建立包含更新版本的套件 __authentication-service__ 使用者。

   _使用套件時，請使用下列臨時因應措施：_

   1. 导航到&#x200B;__工具 > 部署 > 包__。
   1. 建立套件
      + 封裝名稱： `Authentication Service`
      + 版本: `1.0.0`
      + 组: `com.your.company`
   1. 編輯新的 __驗證服務金鑰存放區__ 封裝。
   1. 選取 __篩選器__ 標籤，並為根路徑新增篩選器 `/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore`.
      + 此 `<AUTHENTICATION SERVICE UUID>` 可瀏覽至 __「工具>安全性>使用者」__，並選取 __authentication-service__ 使用者。 UUID是URL的最後一部分。
   1. 選取 __完成__ 然後 __儲存__.
   1. 選取 __建置__ 「 」的按鈕 __驗證服務金鑰存放區__ 封裝。
   1. 建置後，選取 __更多__ > __復寫__ 以將Authentication Service金鑰存放區啟動至AEM Publish。

## 安裝AEM公開/私密金鑰組{#install-aem-public-private-key-pair}

_可選擇安裝AEM公開/私密金鑰組_

AEM Publish可設定為簽署AuthnRequests (to IDP)及加密SAML宣告(to AEM)。 這是透過提供私密金鑰至AEM Publish以及讓公開金鑰與IDP相符來達成。

+++ 瞭解AuthnRequest簽署流程（選用）

AuthnRequest （起始登入程式的AEM Publish向IDP提出的請求）可由AEM Publish簽署。 為此，AEM Publish會使用私密金鑰對AuthnRequest進行簽署，讓IDP接著使用公開金鑰來驗證簽名。 這可向IDP保證AuthnRequest已啟動，並由AEM Publish請求，而不是惡意的第三方。

![SAML 2.0 - SP AuthnRequest簽署](./assets/saml-2-0/sp-authnrequest-signing-diagram.png)

1. 使用者向AEM Publish發出HTTP請求，結果向IDP發出SAML驗證請求。
1. AEM Publish會產生要傳送給IDP的SAML請求。
1. AEM Publish會使用AEM私密金鑰簽署SAML請求。
1. AEM Publish會啟動AuthnRequest，這是一個HTTP使用者端重新導向，指向包含已簽署SAML請求的IDP。
1. IDP會收到AuthnRequest，並使用AEM公開金鑰來驗證簽名，保證AEM Publish已起始AuthnRequest。
1. 然後，AEM Publish會使用IDP公開憑證來驗證解密的SAML宣告的完整性和真實性。

+++

+++ 瞭解SAML宣告加密流程（選擇性）

IDP和AEM Publish之間的所有HTTP通訊都應透過HTTPS，因此預設是安全的。 不過，如有需要，SAML宣告可以加密，以備在HTTPS提供之上需要額外的機密性時使用。 為此，IDP會使用私密金鑰加密SAML宣告資料，而AEM Publish會使用私密金鑰解密SAML宣告。

![SAML 2.0 - SP SAML宣告加密](./assets/saml-2-0/sp-samlrequest-encryption-diagram.png)

1. 使用者向IDP進行驗證。
1. IDP會產生包含使用者資料的SAML宣告，並使用IDP的私人憑證加以簽署。
1. 然後IDP會使用AEM公開金鑰加密SAML宣告，這需要使用AEM私密金鑰來解密。
1. 加密的SAML宣告會透過使用者的網頁瀏覽器傳送至AEM Publish。
1. AEM Publish會收到SAML判斷提示，並使用AEM私密金鑰加以解密。
1. IDP會提示使用者進行驗證。

+++

AuthnRequest簽署和SAML判斷提示加密都是選擇性的，但是兩者都是啟用的，使用 [SAML 2.0驗證處理常式OSGi設定屬性 `useEncryption`](#saml-20-authenticationsaml-2-0-authentication)，表示兩者皆可使用或兩者皆不使用。

![AEM驗證服務金鑰存放區](./assets/saml-2-0/authentication-service-key-store.png)

1. 取得用來簽署AuthnRequest以及加密SAML宣告的公開金鑰、私密金鑰（DER格式為PKCS#8）和憑證鏈結檔案（這可以是公開金鑰）。 這些金鑰通常由IT組織的安全團隊提供。

   + 可使用以下專案產生自我簽署金鑰組： __openssl__：

   ```
   $ openssl req -x509 -sha256 -days 365 -newkey rsa:4096 -keyout aem-private.key -out aem-public.crt
   
   # Provide a password (keep in safe place), and other requested certificate information
   
   # Convert the keys to AEM's required format 
   $ openssl rsa -in aem-private.key -outform der -out aem-private.der
   $ openssl pkcs8 -topk8 -inform der -nocrypt -in aem-private.der -outform der -out aem-private-pkcs8.der
   ```

1. 將公開金鑰上傳至IDP。
   + 使用 `openssl` 方法上，公開金鑰為 `aem-public.crt` 檔案。
1. 以AEM管理員身分登入AEM作者以上傳私密金鑰。
1. 導覽至 __「工具>安全性>信任存放區」__，並選取 __authentication-service__ 使用者，並選取 __屬性__ 從頂端動作列取得。
1. 導覽至 __「工具>安全性>使用者」__，並選取 __authentication-service__ 使用者，並選取 __屬性__ 從頂端動作列取得。
1. 選取 __金鑰存放區__ 標籤。
1. 建立或開啟金鑰存放區。 如果建立金鑰存放區，請妥善保管密碼。
1. 選取 __從DER檔案新增私密金鑰__，並將私密金鑰和鏈結檔案新增至AEM：
   + __別名__：提供有意義的名稱，通常是IDP的名稱。
   + __私密金鑰檔案__：上傳私密金鑰檔案（DER格式的PKCS#8）。
      + 使用 `openssl` 方法，這是 `aem-private-pkcs8.der` 檔案
   + __選取憑證鏈結檔案__：上傳隨附的鏈結檔案（可能是公開金鑰）。
      + 使用 `openssl` 方法，這是 `aem-public.crt` 檔案
   + 選取 __提交__
1. 新新增的憑證會顯示在 __從CRT檔案新增憑證__ 區段。
   + 記下 __別名__ 因為這用於 [SAML 2.0驗證處理常式OSGi設定](#saml-20-authentication-handler-osgi-configuration)
1. 選取 __儲存並關閉__.
1. 建立包含更新版本的套件 __authentication-service__ 使用者。

   _使用套件時，請使用下列臨時因應措施：_

   1. 导航到&#x200B;__工具 > 部署 > 包__。
   1. 建立套件
      + 封裝名稱： `Authentication Service`
      + 版本: `1.0.0`
      + 组: `com.your.company`
   1. 編輯新的 __驗證服務金鑰存放區__ 封裝。
   1. 選取 __篩選器__ 標籤，並為根路徑新增篩選器 `/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore`.
      + 此 `<AUTHENTICATION SERVICE UUID>` 可瀏覽至 __「工具>安全性>使用者」__，並選取 __authentication-service__ 使用者。 UUID是URL的最後一部分。
   1. 選取 __完成__ 然後 __儲存__.
   1. 選取 __建置__ 「 」的按鈕 __驗證服務金鑰存放區__ 封裝。
   1. 建置後，選取 __更多__ > __復寫__ 以將Authentication Service金鑰存放區啟動至AEM Publish。

## 設定SAML 2.0驗證處理常式{#configure-saml-2-0-authentication-handler}

AEM SAML設定是透過 __AdobeGranite SAML 2.0驗證處理常式__ OSGi設定。
此設定是OSGi工廠設定，表示單一AEMas a Cloud Service發佈服務可能有多個SAML設定，涵蓋存放庫的分散資源樹狀結構；這對多網站AEM部署很有用。

+++ SAML 2.0驗證處理常式OSGi設定字彙表

### AdobeGranite SAML 2.0驗證處理常式OSGi設定{#configure-saml-2-0-authentication-handler-osgi-configuration}

|  | OSGi屬性 | 必填 | 值格式 | 預設值 | 描述 |
|-----------------------------------|-------------------------------|:--------:|:---------------------:|---------------------------|-------------|
| 路径 | `path` | ✔ | 字串陣列 | `/` | 用於此驗證處理常式的AEM路徑。 |
| IDP URL | `idpUrl` | ✔ | 字符串 |  | 已傳送SAML驗證請求的IDP URL。 |
| IDP憑證別名 | `idpCertAlias` | ✔ | 字符串 |  | 在AEM全域信任存放區中找到的IDP憑證別名 |
| idp HTTP重新導向 | `idpHttpRedirect` | ✘ | 布尔值 | `false` | 指出是否有HTTP重新導向至IDP URL，而非傳送AuthnRequest。 設定為 `true` 用於IDP啟動的驗證。 |
| IDP識別碼 | `idpIdentifier` | ✘ | 字符串 |  | 唯一的IDP ID可確保AEM使用者和群組的唯一性。 如果為空， `serviceProviderEntityId` 會改用。 |
| 判斷提示消費者服務URL | `assertionConsumerServiceURL` | ✘ | 字符串 |  | 此 `AssertionConsumerServiceURL` AuthnRequest中的URL屬性，指定 `<Response>` 訊息必須傳送至AEM。 |
| SP實體ID | `serviceProviderEntityId` | ✔ | 字符串 |  | 向IDP唯一識別AEM；通常是AEM主機名稱。 |
| SP加密 | `useEncryption` | ✘ | 布尔值 | `true` | 指示IDP是否加密SAML宣告。 需要 `spPrivateKeyAlias` 和 `keyStorePassword` 待設定。 |
| SP私密金鑰別名 | `spPrivateKeyAlias` | ✘ | 字符串 |  | 中私密金鑰的別名 `authentication-service` 使用者的金鑰存放區。 在以下情況下需要 `useEncryption` 設為 `true`. |
| SP金鑰庫密碼 | `keyStorePassword` | ✘ | 字符串 |  | &#39;authentication-service&#39;使用者金鑰庫的密碼。 在以下情況下需要 `useEncryption` 設為 `true`. |
| 預設重新導向 | `defaultRedirectUrl` | ✘ | 字符串 | `/` | 成功驗證後的預設重新導向URL。 可以是相對於AEM主機(例如， `/content/wknd/us/en/html`)。 |
| 使用者ID屬性 | `userIDAttribute` | ✘ | 字符串 | `uid` | 包含AEM使用者之使用者ID的SAML宣告屬性名稱。 留空將使用 `Subject:NameId`. |
| 自動建立AEM使用者 | `createUser` | ✘ | 布尔值 | `true` | 表示是否會在成功驗證時建立AEM使用者。 |
| AEM使用者中繼路徑 | `userIntermediatePath` | ✘ | 字符串 |  | 建立AEM使用者時，此值會作為中繼路徑(例如， `/home/users/<userIntermediatePath>/jane@wknd.com`)。 需要 `createUser` 將設為 `true`. |
| AEM使用者屬性 | `synchronizeAttributes` | ✘ | 字串陣列 |  | 要儲存在AEM使用者上的SAML屬性對應清單，格式為 `[ "saml-attribute-name=path/relative/to/user/node" ]` (例如， `[ "firstName=profile/givenName" ]`)。 請參閱 [原生AEM屬性的完整清單](#aem-user-attributes). |
| 新增使用者至AEM群組 | `addGroupMemberships` | ✘ | 布尔值 | `true` | 表示在成功驗證後，AEM使用者是否自動新增到AEM使用者群組。 |
| AEM群組成員資格屬性 | `groupMembershipAttribute` | ✘ | 字符串 | `groupMembership` | SAML宣告屬性的名稱，其中包含使用者應新增至的AEM使用者群組清單。 需要 `addGroupMemberships` 將設為 `true`. |
| 預設AEM群組 | `defaultGroups` | ✘ | 字串陣列 |  | 已驗證身分的AEM使用者群組清單會一律新增至(例如， `[ "wknd-user" ]`)。 需要 `addGroupMemberships` 將設為 `true`. |
| NameIDPolicy格式 | `nameIdFormat` | ✘ | 字符串 | `urn:oasis:names:tc:SAML:2.0:nameid-format:transient` | 要在AuthnRequest訊息中傳送的NameIDPolicy格式引數值。 |
| 儲存SAML回應 | `storeSAMLResponse` | ✘ | 布尔值 | `false` | 顯示 `samlResponse` 值會儲存在AEM上 `cq:User` 節點。 |
| 處理登出 | `handleLogout` | ✘ | 布尔值 | `false` | 指出此SAML驗證處理常式是否處理登出要求。 需要 `logoutUrl` 待設定。 |
| 登出URL | `logoutUrl` | ✘ | 字符串 |  | IDP的URL，將SAML登出請求傳送至此URL。 在以下情況下需要 `handleLogout` 設為 `true`. |
| 時鐘公差 | `clockTolerance` | ✘ | 整数 | `60` | 驗證SAML宣告時，IDP和AEM (SP)時鐘扭曲容許度。 |
| 摘要方法 | `digestMethod` | ✘ | 字符串 | `http://www.w3.org/2001/04/xmlenc#sha256` | IDP在簽署SAML訊息時使用的摘要演演算法。 |
| 簽章方法 | `signatureMethod` | ✘ | 字符串 | `http://www.w3.org/2001/04/xmldsig-more#rsa-sha256` | IDP在簽署SAML訊息時使用的簽章演演算法。 |
| 身分同步型別 | `identitySyncType` | ✘ | `default` 或 `idp` | `default` | 不要變更 `from` AEMas a Cloud Service的預設。 |
| 服務排名 | `service.ranking` | ✘ | 整数 | `5002` | 相同專案偏好排名較高的設定 `path`. |

### AEM使用者屬性{#aem-user-attributes}

AEM會使用以下使用者屬性，這些屬性可透過 `synchronizeAttributes` 屬性(在AdobeGranite SAML 2.0 Authentication Handler OSGi設定中)。  任何IDP屬性都可以同步至任何AEM使用者屬性，但是對應至AEM使用屬性屬性（如下所列）可讓AEM自然地使用它們。

| 使用者屬性 | 相對屬性路徑 `rep:User` 節點 |
|--------------------------------|--------------------------|
| 標題(例如， `Mrs`) | `profile/title` |
| 名字（即名字） | `profile/givenName` |
| 姓氏 | `profile/familyName` |
| 職稱 | `profile/jobTitle` |
| 电子邮件地址 | `profile/email` |
| 街道地址 | `profile/street` |
| 城市 | `profile/city` |
| 郵遞區號 | `profile/postalCode` |
| 国家/地区 | `profile/country` |
| 電話號碼 | `profile/phoneNumber` |
| 自我介绍 | `profile/aboutMe` |

+++

1. 在專案中建立OSGi設定檔案： `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` 並在IDE中開啟。
   + 變更 `/wknd-examples/` 至您的 `/<project name>/`
   + 之後的識別碼 `~` 檔案名稱中應唯一識別此設定，因此它可能是IDP的名稱，例如 `...~okta.cfg.json`. 值應為英數字元和連字型大小。
1. 將下列JSON貼入 `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json` 檔案並更新 `wknd` 視需要參考。

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

1. 依專案要求更新值。 請參閱 __SAML 2.0驗證處理常式OSGi設定字彙表__ 以上為設定屬性說明
1. 值可能會隨著發行週期不同步變更，或值在類似環境型別/服務層級之間不同時，建議使用OSGi環境變數和秘密，但並非必要。 可使用以下專案設定預設值： `$[env:..;default=the-default-value]"` 語法，如上所示。

每個環境的OSGi設定(`config.publish.dev`， `config.publish.stage`、和 `config.publish.prod`)如果SAML設定在不同環境之間有所不同，則可以使用特定屬性來定義。

### 使用加密

時間 [加密AuthnRequest和SAML判斷提示](#encrypting-the-authnrequest-and-saml-assertion)，則需要下列屬性： `useEncryption`， `spPrivateKeyAlias`、和 `keyStorePassword`. 此 `keyStorePassword` 包含密碼，因此值不能儲存在OSGi設定檔案中，而是使用插入 [密碼設定值](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#secret-configuration-values)

+++可選擇更新OSGi設定以使用加密

1. 開啟 `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` 在IDE中。
1. 新增三個屬性 `useEncryption`， `spPrivateKeyAlias`、和 `keyStorePassword` 如下所示。

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

1. 加密所需的三個OSGi設定屬性是：

+ `useEncryption` 設定為 `true`
+ `spPrivateKeyAlias` 包含SAML整合所使用之私密金鑰的金鑰庫專案別名。
+ `keyStorePassword` 包含 [OSGi密碼設定變數](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#secret-configuration-values) 包含 `authentication-service` 使用者金鑰庫的密碼。

+++

## 設定反向連結篩選

在SAML驗證程式期間，IDP會向AEM Publish的使用者端HTTPPOST `.../saml_login` 端點。 如果IDP和AEM Publish存在於不同的來源，則AEM Publish的 __反向連結篩選__ 是透過OSGi設定設定以允許來自IDP來源的HTTP POST。

1. 建立（或編輯）專案中的OSGi設定檔，網址為 `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/org.apache.sling.security.impl.ReferrerFilter.cfg.json`.
   + 變更 `/wknd-examples/` 至您的 `/<project name>/`
1. 確保 `allow.empty` 值設定為 `true`，則 `allow.hosts` (或如果您願意， `allow.hosts.regexp`)包含IDP的來源，以及 `filter.methods` 包含 `POST`. OSGi設定應該類似於：

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

AEM Publish支援單一反向連結篩選設定，因此可將SAML設定需求與任何現有設定合併。

每個環境的OSGi設定(`config.publish.dev`， `config.publish.stage`、和 `config.publish.prod`)可使用特定屬性定義，如果 `allow.hosts` (或 `allow.hosts.regex`)因環境而異。

## 設定跨原始資源共用(CORS)

在SAML驗證程式期間，IDP會向AEM Publish的使用者端HTTPPOST `.../saml_login` 端點。 如果IDP和AEM Publish存在於不同的主機/網域中，則AEM Publish的 __CRoss-Origin資源共用(CORS)__ 必須設定為允許來自IDP主機/網域的HTTP POST。

此HTTPPOST請求的 `Origin` 標頭的值通常與AEM Publish主機不同，因此需要CORS設定。

在本機AEM SDK上測試SAML驗證時(`localhost:4503`)，則IDP可將 `Origin` 頁首至 `null`. 如果是，請新增 `"null"` 至 `alloworigin` 清單。

1. 在專案中建立OSGi設定檔案： `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~saml.cfg.json`
   + 變更 `/wknd-examples/` 至您的專案名稱
   + 之後的識別碼 `~` 檔案名稱中應唯一識別此設定，因此它可能是IDP的名稱，例如 `...CORSPolicyImpl~okta.cfg.json`. 值應為英數字元和連字型大小。
1. 將下列JSON貼入 `com.adobe.granite.cors.impl.CORSPolicyImpl~...cfg.json` 檔案。

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

每個環境的OSGi設定(`config.publish.dev`， `config.publish.stage`、和 `config.publish.prod`)可使用特定屬性定義，如果 `alloworigin` 和 `allowedpaths` 因環境而異。

## 設定AEM Dispatcher以允許SAML HTTP POST

成功驗證IDP後，IDP會將HTTPPOST協調回AEM註冊的頁面 `/saml_login` 端點（設定於IDP）。 此HTTPPOST至 `/saml_login` Dispatcher預設會加以封鎖，因此必須使用下列Dispatcher規則明確加以允許：

1. 開啟 `dispatcher/src/conf.dispatcher.d/filters/filters.any` 在IDE中。
1. 在檔案底部新增允許規則，將HTTP POST新增至結尾為 `/saml_login`.

```
...

# Allow SAML HTTP POST to ../saml_login end points
/0190 { /type "allow" /method "POST" /url "*/saml_login" }
```

如果已設定Apache Webserver上的URL重寫(`dispatcher/src/conf.d/rewrites/rewrite.rules`)，確定向以下專案的請求： `.../saml_login` 不會意外損壞端點。

## 啟用資料同步並封裝Token

在SAML驗證流程於AEM Publish中建立使用者後，AEM使用者節點即可在整個AEM Publish服務層級中進行驗證。
這需要 [資料同步](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html#data-synchronization) 和 [封裝的Token](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html#sticky-sessions-and-encapsulated-tokens) 將由AEM Publish服務的Adobe支援啟用。

傳送要求給Adobe客戶支援(透過 [AdminConsole](https://adminconsole.adobe.com) >支援)要求：

> 方案X和環境Y的AEM Publish服務已啟用資料同步和封裝權杖。

## 部署SAML設定

OSGi設定必須提交到Git並使用Cloud Manager部署到AEMas a Cloud Service。

```
$ git remote -v            
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (fetch)
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (push)
$ git add .
$ git commit -m "SAML 2.0 configurations"
$ git push adobe saml-auth:develop
```

部署目標Cloud Manager Git分支(在此範例中， `develop`)，使用完整棧疊部署管道。
