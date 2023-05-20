---
title: 與外部應用程式as a Cloud Service向AEM進行驗證
description: 探索外部應用程式如何使用本機開發存取權杖和服務認證，以程式設計方式透過HTTP驗證並與AEMas a Cloud Service互動。
version: Cloud Service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330460.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
exl-id: 63c23f22-533d-486c-846b-fae22a4d68db
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '641'
ht-degree: 0%

---

# 對AEMas a Cloud Service的權杖型驗證

AEM公開各種能以無周邊方式互動的HTTP端點，從GraphQL、AEM Content Services到Assets HTTP API。 通常，這些Headless消費者可能需要向AEM驗證身分，才能存取受保護的內容或動作。 為方便起見，AEM支援對來自外部應用程式、服務或系統的HTTP請求進行權杖式驗證。

在本教學課程中，請深入探索外部應用程式如何以程式設計方式使用存取權杖並透過HTTP向AEMas a Cloud Service驗證及互動。

>[!VIDEO](https://video.tv.adobe.com/v/330460?quality=12&learn=on)

## 先決條件

在執行本教學課程之前，請確定已具備下列專案：

1. 存取AEMas a Cloud Service環境（最好是開發環境或沙箱計畫）
1. AEMas a Cloud Service環境作者服務AEM管理員產品設定檔的成員資格
1. 擁有Adobe IMS組織管理員的會員資格或存取權(他們將必須執行 [服務認證](./service-credentials.md))
1. 最新 [WKND網站](https://github.com/adobe/aem-guides-wknd) 部署至您的Cloud Service環境

## 外部應用程式概述

本教學課程使用 [簡單Node.js應用程式](./assets/aem-guides_token-authentication-external-application.zip) 從命令列執行，以使用更新AEMas a Cloud Service的資產中繼資料 [Assets HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html).

Node.js應用程式的執行流程如下：

![外部應用計畫](./assets/overview/external-application.png)

1. 從命令列叫用Node.js應用程式
1. 命令列引數定義：
   + 要連線的AEMas a Cloud Service作者服務主機(`aem`)
   + 資產已更新的AEM資產資料夾(`folder`)
   + 要更新的中繼資料屬性和值(`propertyName` 和 `propertyValue`)
   + 檔案的本機路徑，提供存取AEMas a Cloud Service所需的認證(`file`)
1. 用於驗證AEM的存取權杖衍生自透過命令列引數提供的JSON檔案 `file`

   a.若用於非本機開發的服務認證提供在JSON檔案中(`file`)，則存取權杖會從Adobe IMS API擷取
1. 應用程式會使用存取權杖來存取AEM，並列出在命令列引數中指定的資料夾中的所有資產 `folder`
1. 對於資料夾中的每個資產，應用程式會根據命令列引數中指定的屬性名稱和值更新其中繼資料 `propertyName` 和 `propertyValue`

雖然此範例應用程式是Node.js，但可以使用不同的程式設計語言開發這些互動，並從其他外部系統執行。

## 本機開發存取權杖

本機開發存取權杖是為特定AEMas a Cloud Service環境產生的，可讓您存取製作和發佈服務。  這些存取權杖為暫時性，僅用於開發透過HTTP與AEM互動的外部應用程式或系統。 開發人員不必取得和管理好記的服務認證，他們可以快速輕鬆地自行產生暫時存取權杖，好讓他們開發整合。

+ [如何使用本機開發存取權杖](./local-development-access-token.md)

## 服務認證

服務憑證是任何非開發情況（最明顯的是生產情況）中使用的真實憑證，可促進外部應用程式或系統通過HTTP向AEMas a Cloud Service進行驗證並與之互動。 服務憑證本身不會傳送到AEM進行驗證，而是由外部應用程式使用這些憑證來產生JWT，並與Adobe IMS的API交換 _的_ 存取權杖，然後可用於向AEMas a Cloud Service驗證HTTP請求。

+ [如何使用服務認證](./service-credentials.md)

## 其他資源

+ [下載應用程式範例](./assets/aem-guides_token-authentication-external-application.zip)
+ JWT建立和交換的其他程式碼範例
   + [Node.js、Java、Python、C#.NET和PHP程式碼範例](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/)
   + [JavaScript/Axios型程式碼範例](https://github.com/adobe/aemcs-api-client-lib)
