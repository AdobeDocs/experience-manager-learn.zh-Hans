---
title: 測試AEM內容片段主控台擴充功能
description: 瞭解如何在部署到生產環境之前測試AEM內容片段主控台擴充功能。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
exl-id: c5c1df23-1c04-4c04-b0cd-e126c31d5acc
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---

# 測試擴充功能

AEM內容片段主控台擴充功能可針對擴充功能所屬的Adobe組織中的任何AEMas a Cloud Service環境進行測試。

擴充功能測試是透過特別建立的URL完成，此URL會指示AEM內容片段主控台載入擴充功能。

>[!VIDEO](https://video.tv.adobe.com/v/3412877?quality=12&learn=on)

## AEM內容片段主控台URL

![AEM內容片段主控台URL](./assets/test/content-fragment-console-url.png){align="center"}

若要建立將非生產擴充功能掛載至AEM內容片段主控台的URL，必須收集所需AEM內容片段主控台的URL。 導覽至AEMas a Cloud Service環境以測試擴充功能，並複製其AEM內容片段主控台的URL。

1. 登入所需的AEMas a Cloud Service環境。

   + 使用AEM開發環境進行 [測試開發組建](#testing-development-builds)
   + 使用AEM Stage或QA開發環境進行 [測試階段組建](#testing-stage-builds)

1. 選取 __內容片段__ 圖示。
1. 等待AEM內容片段主控台載入瀏覽器。
1. 從瀏覽器的位址列複製AEM內容片段主控台的URL，應該類似於：

   ```
   https://experience.adobe.com/?repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

以下為開發和中繼測試精心設計URL時，會使用此URL。

## 測試本機開發組建

1. 開啟擴充功能專案根目錄的命令列。
1. 以本機App Builder應用程式身分執行AEM內容片段主控台擴充功能

   ```shell
   $ aio app run
   ...
   No change to package.json was detected. No package manager install will be executed.
   
   To view your local application:
     -> https://localhost:9080
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://localhost:9080
   ```

記下本機應用程式URL，如下所示 `-> https://localhost:9080`

1. 將下列兩個查詢引數新增至 [AEM內容片段主控台的URL](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<LOCAL APPLICATION URL>`，通常 `&ext=https://localhost:9080`.

   新增上述兩個查詢引數(`devMode` 和 `ext`)作為 __第一個__ URL中的查詢引數，因為內容片段主控台使用雜湊路由(`#/@wknd/aem/...`)，因此在 `#` 將無法運作。

   測試URL應如下所示：

   ```
   https://experience.adobe.com/?devMode=true&ext=https://localhost:9080&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. 將測試URL複製並貼到瀏覽器中。

   + 您可能必須先進行初始設定，然後再定期設定，您必須 [接受HTTPS憑證](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#accepting-the-certificate-first-time-users) 本機應用程式的主機(`https://localhost:9080`)。

1. AEM內容片段主控台會載入並插入其本機版本的擴充功能以進行測試，而只要本機App Builder應用程式執行中，就會熱重新載入變更。

>[!IMPORTANT]
>
>請記住，使用此方法時，開發中的擴充功能只會影響您的體驗，而AEM內容片段主控台的所有其他使用者無需插入擴充功能即可存取擴充功能。


## 測試階段組建

1. 開啟擴充功能專案根目錄的命令列。
1. 請確定Stage工作區作用中（或任何用於測試的工作區）。

   ```shell
   $ aio app use -w Stage
   ```

   將任何變更合併至 `.env` 和 `.aio`.

1. 部署更新的擴充功能App Builder應用程式。 如果未登入，請執行 `aio login` 首先。

   ```shell
   $ aio app deploy
   ...
   Your deployed actions:
   web actions:
     -> https://98765-123aquarat.adobeio-static.net/api/v1/web/aem-cf-console-admin-1/generic 
   To view your deployed application:
     -> https://98765-123aquarat.adobeio-static.net/index.html
   To view your deployed application in the Experience Cloud shell:
     -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://98765-123aquarat.adobeio-static.net/index.html
   New Extension Point(s) in Workspace 'Production': 'aem/cf-console-admin/1'
   Successful deployment 🏄
   ```

1. 將下列兩個查詢引數新增至 [AEM內容片段主控台的URL](#aem-content-fragment-console-url)
   + `&devMode=true`
   + `&ext=<DEPLOYED APPLICATION URL>`

   新增上述兩個查詢引數(`devMode` 和 `ext`)作為 __第一個__ URL中的查詢引數，因為內容片段主控台使用雜湊路由(`#/@wknd/aem/...`)，因此在 `#` 將無法運作。

   測試URL應如下所示：

   ```
   https://experience.adobe.com/?devMode=true&ext=https://98765-123aquarat.adobeio-static.net/index.html&repo=author-p1234-e5678.adobeaemcloud.com#/@wknd/aem/cf/admin
   ```

1. 將測試URL複製並貼到瀏覽器中。
1. AEM內容片段主控台會插入在中部署至中繼工作區的擴充功能版本。 此Stage URL可與QA或企業使用者共用，以進行測試。

請記住，使用此方法時，只有在透過製作階段URL存取時，階段擴充功能才會插入到AEM內容片段主控台的。

1. 部署後的擴充功能可透過執行 `aio app deploy` 再次強調，這些變更會在使用測試URL時自動反映。
1. 若要移除擴充功能以進行測試，請執行 `aio app undeploy`.
