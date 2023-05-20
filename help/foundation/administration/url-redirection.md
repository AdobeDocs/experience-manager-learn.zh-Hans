---
title: URL重新導向
description: 瞭解在AEM中執行URL重新導向的各種選項。
version: 6.4, 6.5, Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer, Architect
level: Intermediate
kt: 11466
last-substantial-update: 2022-10-14T00:00:00Z
index: y
exl-id: 8e64f251-e5fd-4add-880e-9d54f8e501a6
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '876'
ht-degree: 1%

---

# URL重新導向

URL重新導向是網站作業中常見的方面。 對於如何以及在何處管理URL重新導向，架構師和管理員必須尋找最佳解決方案，以提供彈性和快速的重新導向部署時間。

請務必熟悉 [AEM (6.x) aka AEM Classic](https://experienceleague.adobe.com/docs/experience-manager-learn/dispatcher-tutorial/chapter-2.html#the-%E2%80%9Clegacy%E2%80%9D-setup) 和 [AEMas a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/architecture.html#runtime-architecture) 基礎結構。 主要差異如下：

1. AEMas a Cloud Service具有 [內建CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html)但是，客戶可以在AEM管理的CDN之前提供CDN (BYOCDN)。
1. AEM 6.x (無論是內部部署或Adobe Managed Services (AMS))不包含AEM管理的CDN，且客戶必須自備。

其他AEM服務（AEM Author/Publish和Dispatcher）在AEM 6.x和AEMas a Cloud Service之間的概念類似。

AEM URL重新導向解決方案如下：

|  | 作為AEM專案程式碼管理及部署 | 可依行銷/內容團隊變更 | AEM as Cloud Service相容 | 發生重新導向執行的位置 |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [At Edge透過自備CDN](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | Edge/CDN |
| [Apache `mod_rewrite` 規則作為Dispatcher設定 ](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ACS Commons — 重新導向地圖管理員](#redirect-map-manager) | ✘ | ✔ | ✘ | Dispatcher |
| [ACS Commons — 重新導向管理員](#redirect-manager) | ✘ | ✔ | ✔ | AEM |
| [此 `Redirect` 頁面屬性](#the-redirect-page-property) | ✘ | ✔ | ✔ | AEM |


## 解決方案選項

以下是解決方案選項，順序為更接近網站訪客的瀏覽器。

### At Edge透過自備CDN

有些CDN服務提供邊緣層級的重新導向解決方案，因此可減少前往原點的來回次數。 另請參閱 [Akamai Edge重新導向程式](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector)， [AWS CloudFront函式](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html). 如需邊緣層級重新導向功能，請洽詢您的CDN服務提供者。

在Edge或CDN層級管理重新導向具有效能優勢，但是這些優勢並非作為AEM的一部分來管理，而是作為獨立專案來管理。 管理及部署重新導向規則的深思熟慮程式對避免問題至關重要。


### Apache `mod_rewrite` 模組

常見的解決方案使用 [Apache模組mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html). 此 [AEM專案原型](https://github.com/adobe/aem-project-archetype) 為兩者提供Dispatcher專案結構 [AEM 6.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure) 和 [AEMas a Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure) 專案。 預設（不可變）和自訂重寫規則定義於 `conf.d/rewrites` 資料夾，而且重新寫入引擎已開啟 `virtualhosts` 在連線埠上接聽的 `80` 透過 `conf.d/dispatcher_vhost.conf` 檔案。 實作範例可在以下取得： [AEM WKND網站專案](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites).

在AEMas a Cloud Service中，這些重新導向規則會作為AEM程式碼的一部分進行管理，並透過Cloud Manager部署 [Web層設定管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#web-tier-config-pipelines) 或 [完整棧疊管道](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#full-stack-pipeline). 因此，您的AEM專案特定程式將用於管理、部署和追蹤重新導向規則。

大部分的CDN服務都會快取HTTP 301和302重新導向，視其 `Cache-Control` 或 `Expires` 標頭。 這有助於避免最初在Apache/Dispatcher處重新導向之後的來回。


### ACS AEM Commons

內有兩項可用的功能 [ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/) 管理URL重新導向。 請注意，ACS AEM Commons是社群運作的開放原始碼專案，Adobe不支援。

#### 重新導向地圖管理員

[重新導向地圖管理員](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) 可讓AEM 6.x管理員輕鬆維護和發佈 [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) 檔案不需直接存取Apache Web Server或要求Apache Web Server重新啟動。 此功能可讓許可權使用者從AEM中的主控台建立、更新和刪除重新導向規則，無需開發團隊或AEM部署協助。 重新導向地圖管理員是 **與AEMas a Cloud Service不相容**.

#### 重新導向管理員

[重新導向管理員](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) 可讓AEM的使用者輕鬆維護和發佈AEM的重新導向。 實作是以Java™ servlet篩選為基礎，因此是典型的JVM資源消耗。 此功能也會消除對AEM開發團隊和AEM部署的相依性。 重新導向管理員是兩者 **AEMas a Cloud Service** 和 **AEM 6.x** 相容。 雖然初始重新導向請求預設必須點選AEM Publish服務來產生301/302 （大多數） CDN的快取301/302，允許後續請求被重新導向到edge/CDN。

### 此 `Redirect` 頁面屬性

開箱即用(OOTB) `Redirect` 頁面屬性 [進階索引標籤](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/fundamentals/page-properties.html#advanced) 可讓內容作者定義目前頁面的重新導向位置。 此解決方案最適合每個頁面重新導向案例，而且沒有可檢視和管理頁面重新導向的中央位置。

## 哪個解決方案適合實施

以下是判斷正確解決方案的一些條件。 此外，貴組織的IT和行銷流程應有助於挑選正確的解決方案。

1. 讓行銷團隊或超級使用者在沒有AEM開發團隊和AEM部署的情況下管理重新導向規則。
1. 管理、驗證、追蹤及回覆變更或降低風險的程式。
1. 的可用性 _主題專業知識_ 的 **At Edge （透過CDN服務）** 解決方案。
