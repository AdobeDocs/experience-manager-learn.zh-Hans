---
title: 瞭解升級的原因
description: 為考慮升級至最新版Adobe Experience Manager 6的客戶提供主要功能的高層級劃分。
version: 6.5
topic: Upgrade
role: Leader, Architect, Developer, Admin, User
level: Beginner
exl-id: bf4030b0-67c4-4b00-af95-f63e6f79e995
source-git-commit: 81b96f59450448a3d5b17a61aa025acd60d0cce1
workflow-type: tm+mt
source-wordcount: '3460'
ht-degree: 3%

---

# 瞭解升級的原因

為考慮升級至最新版Adobe Experience Manager 6的客戶提供主要功能的高層級劃分。

## 升級至AEM 6.5的主要功能

+ [Adobe Experience Manager 6.5發行說明](https://helpx.adobe.com/cn/experience-manager/6-5/release-notes.html)

### 基礎改良

Adobe Experience Manager 6.5透過下列方式，持續增強系統的穩定性、效能和支援能力：

+ **Java 11** 支援（同時維持Java 8支援）。

### 網站建立與管理

AEM Sites推出多項功能，旨在加速網站的建立和建置：

+ **SPA編輯器** 支援可讓SPA （單頁應用程式）在AEM中完全撰寫，支援豐富、方便行銷人員的撰寫體驗。
+_ **JavaScript SDK的**&#x200B;是一套SPA專案入門套件和支援的建置工具，可讓前端開發人員獨立於AEM開發與SPA編輯器相容的單頁應用程式。
+ **核心元件** 新增許多新元件，例如 **元件資料庫** 以及對現有核心元件的各種增強功能。
+ 進一步的 **翻譯** 增強功能可簡化AEM Sites的翻譯。

### 流暢的使用體驗

AEM持續採用流動體驗，提供全新及改良的工具，協助您使用AEM以外的內容。

+ **內容片段** 支援版本比較/比較和註解。
+ **AEM Assets HTTP API** 支援公開 **內容片段** 直接在DAM中作為 **JSON**.
   **體驗片段** 支援 **全文搜尋** 和 **AEM Dispatcher快取失效** 以供參考 **頁面**.

### 資產管理

AEM Assets繼續以其豐富的資產管理功能集為基礎進行擴充，以改進對DAM的使用、管理和瞭解。 AEM 6.5持續改善Adobe Creative Cloud與創意工作流程之間的整合。

+ **Adobe資產連結** 從Adobe Creative Cloud工具將創意內容直接連線至AEM Assets。
+ **Adobe Stock** 整合可讓您直接從AEM Assets體驗存取Adobe Stock影像，創造順暢的內容探索體驗。
+ **AEM案頭應用程式** 發行版本2.0，在改善效能和穩定性的同時重新構想自身。
+ **連線資產** 支援獨立的AEM Sites執行個體，以便順暢地存取和使用其他AEM Assets執行個體的資產。
+ 更新中的視訊支援 **Dynamic Media**，包括 **360度影片** 和 **自訂視訊縮圖**.

### 內容智慧

AEM持續與智慧技術建立整合，利用機器學習和人工智慧來改善所有體驗。

+ **Adobe資產連結** 新增 **視覺相似性搜尋**，可讓您輕鬆探索和使用類似影像 **Adobe Creative Cloud工具**.

### 集成

AEM提升與其他Adobe服務整合的能力：

+ **體驗片段** 深化與的整合 **Adobe Target** 支援 **匯出為JSON** 至Adobe Target以及以下功能 **刪除體驗片段型選件** 從 **Adobe Target**.

### AMS Cloud Manager

[Cloud Manager](https://adobe.ly/2HODmsv)是Adobe Managed Services (AMS)客戶的專屬產品，提供下列功能：

+ Cloud Manager支援將AEM部署支援從AEM Sites擴充到 **AEM Assets**，包括 **資產處理的自動化效能測試**.
+ **自動縮放** 達到預先定義的臨界值的AEM Publish層級，確保最佳的一般使用者體驗。
+ **非生產管道** 允許開發團隊利用Cloud Manager持續檢查計畫碼品質並部署到較低環境（開發和QA）。
+ **CI/CD管道API** 允許客戶以程式設計方式與Cloud Manager互動，深化與內部部署開發基礎結構的整合可能性。

## 基礎功能

以下為AEM所提供的重要基礎功能矩陣。 其中部分功能已在每個版本的舊版中新增的增量增強功能中引入。

+ [AEM Foundation發行說明](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***✔<sup>+</sup> 此版本大幅增強功能。***

***✔<sup>SP</sup> 表示此功能可透過Service Pack或Feature Pack取得。***

<table>
    <thead>
        <tr>
            <td>基礎功能</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <strong>Java 11支援：</strong> AEM支援Java 11 （以及Java 8）。
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Oak內容存放庫</a>：</strong> 比前身的Jackrabbit 2提供更優異的效能和擴充性。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">oak-run.jar索引支援</a>：</strong> 改善Oak索引的重新索引、統計資料收集和一致性檢查。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">自訂搜尋索引</a>： </strong>
                可新增自訂索引定義，以最佳化查詢效能和搜尋相關性。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/cn/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">線上修訂清除</a>：</strong>
                執行存放庫維護作業，避免伺服器停機。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">TarMK或MongoMK存放庫儲存</a>：</strong>
                <br> 可選擇使用簡單、高效能的TarMK檔案式儲存（新一代的TarPM版本）
                <br> 或使用MongoDB支援的存放庫和MongoMK水平縮放。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">MongoMK效能與穩定性</a>：</strong>
            自MongoMK推出AEM 6.0以來，已持續增強功能。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">Amazon S3 DataStore</a>：</strong>
            運用可擴充的雲端儲存解決方案來儲存二進位資產。</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Touch UI功能比較：</strong>
                持續增強編寫UI的速度，並透過傳統UI提高生產力和功能對等性。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Omnisearch：</strong>
                快速搜尋和導覽AEM。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">操作控制面板</a>：</strong>
 從AEM內執行維護、監控伺服器健康狀態並分析效能。</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">升級改善</a>：</strong>
            升級改良功能可讓您更輕鬆、更快速地就地升級AEM。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/cn/experience-manager/htl/using/overview.html" target="_blank">HTL範本語言</a>：</strong>
            將表示與邏輯分隔的現代範本引擎。 大幅縮短元件開發時間。 每個版本都新增了增量功能。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Sling模型</a>：</strong>
            將JCR資源模型化為商業物件和邏輯的彈性架構。 每個版本都新增了增量功能。
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">Cloud Manager</a>： </strong>
                Adobe Managed Services (AMS)客戶專有，Cloud Manager透過最先進的CI/CD管道加速開發和部署。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## 安全性功能

以下為AEM所提供之主要安全性功能的對照表。 其中部分功能已在每個版本的舊版中新增的增量增強功能中引入。

+ [安全性發行說明](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***✔表示此版本中的功能有重大增強功能。***

***✔<sup>+</sup> 表示此功能可透過Service Pack或Feature Pack取得。***

<table>
    <thead>
        <tr>
            <td>安全性功能</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">服務使用者</a></strong>
            <br> 劃分許可權以避免不必要地使用管理員許可權。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/cn/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">金鑰庫管理</a></strong>
            <br> 全域信任存放區、憑證和金鑰都在存放庫中管理。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong>CSRF</strong> <strong>保護</strong></a>
            <br> 立即可用的跨網站請求偽造保護。</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/cn/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>CORS</strong> <strong>支援</strong></a>
            <br> 跨原始資源共用支援，可提供更優異的應用程式彈性。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://experienceleague.adobe.com/docs/" target="_blank">改善SAML驗證支援</a><br>
 </strong>改善SAML重新導向、最佳化的群組資訊和金鑰加密問題。
            <br>
        </td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP作為OSGi設定</a><br>
 </strong>簡化LDAP驗證的管理和更新。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong>純文字密碼的OSGi加密支援<br>
 </strong>密碼和其他敏感值可以加密形式儲存並自動解密。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">CUG增強功能</a><br>
 </strong>封閉式使用者群組實作已重新寫入，以解決效能和擴充性問題。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔<sup>+</sup></td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">SSL精靈</a></strong>
            <br> UI以簡化SSL的設定和管理。</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">封裝權杖支援</a></strong>
            <br> 「粘性」工作階段不再需要支援跨發佈執行個體的水準驗證。</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Adobe IMS驗證支援</a><br>
 </strong>Adobe Managed Services (AMS)專屬，透過Adobe IMS (Identity Management System)集中管理AEM作者執行個體的存取權。</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
    </tr>
</tbody>
</table>

## Sites功能

以下為AEM所提供之主要Sites功能的矩陣。 其中部分功能已在每個版本的舊版中新增的增量增強功能中引入。

+ [AEM Sites發行說明](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***✔<sup>+</sup> 此版本大幅增強功能。***

***✔<sup>SP</sup> 表示此功能可透過Service Pack或Feature Pack取得。***

<table>
    <thead>
        <tr>
            <td><strong>Sites功能</strong></td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">觸控最佳化的頁面製作</a>：</strong>
            可讓編輯器運用平板電腦和配備觸控熒幕的電腦。</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">回應式網站編寫</a>：</strong>
                版面模式可讓編輯器根據回應式網站的裝置寬度調整元件大小。</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">可編輯的範本</a>：</strong>
            允許專門的作者建立和編輯頁面範本。</td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/core-components/user-guide.html" target="_blank">核心元件</a>：</strong>
            加速網站開發。 GitHub提供頻繁發行排程和彈性。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">SPA編輯器</a>：</strong>
            使用以React為基礎建立的單頁應用程式(SPA)架構，建立可編寫、可啟動的網頁體驗。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>樣式系統：</strong>
            透過使用上下文內樣式系統定義其視覺外觀，增加AEM元件的重複使用率。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/cn/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">多站點管理員(MSM)</a>：</strong>
            管理共用相同內容的多個網站（即多語言、多個品牌）。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/cn/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">內容翻譯</a>：</strong>
            隨插即用架構整合了業界領先的第三方翻譯服務。</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank">ContextHub</a>：</strong>
            用於個人化內容的新一代使用者端內容架構。</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">啟動</a>：</strong>
            在不中斷日常製作的情況下，為未來版本開發內容。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>內容片段：</strong>
            建立和組織從簡報中分離的編輯內容，以便重複使用。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank">體驗片段</a>：</strong>
            建立針對案頭、行動裝置和社交頻道最佳化的可重複使用體驗和變數。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>內容服務：</strong>
            從AEM將內容匯出為JSON，以便跨裝置和應用程式使用。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Adobe Analytics整合與內容深入分析：</strong>
                輕鬆整合Adobe Analytics和DTM。 在作者環境中顯示效能資訊。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Adobe Target整合</a>：</strong>
            逐步精靈可建立鎖定目標的體驗，以及可重複使用的選件程式庫。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Adobe Campaign整合</a>：</strong>
            與新一代電子郵件行銷活動解決方案輕鬆整合。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/cn/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">Adobe Launch整合</a>：</strong>
            整合Adobe的新一代標籤管理雲端服務。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>畫面：</strong>
            管理數位看板和資訊站體驗。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">電子商務</a>：</strong>
            在網路、行動裝置和社交接觸點之間，提供品牌和個人化的購物體驗。
            </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html" target="_blank">Communities</a>：</strong>
            論壇、執行緒評論、活動行事曆和許多其他功能都允許與網站訪客進行深入互動。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Assets功能

以下為AEM所提供之主要Assets功能的矩陣。 其中部分功能已在每個版本的舊版中新增的增量增強功能中引入。

+ [AEM Assets發行說明](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***✔表示此版本中的功能有重大增強功能。***

***✔<sup>+</sup> 表示此功能可透過Service Pack或Feature Pack取得。***

<table>
    <thead>
        <tr>
            <td>Assets功能</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">觸控最佳化的UI</a>：</strong>
            在桌上型電腦或觸控式裝置上管理資產。</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/metadata.html" target="_blank">進階中繼資料管理</a>：</strong>
            中繼資料範本、中繼資料結構編輯器和大量中繼資料編輯。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank">任務</a> 和 <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank">工作流程</a> 管理：</strong>
            運用AEM專案稽核和核准數位資產的預先建立工作流程和任務。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>擴充性與效能：</strong>
            增強對大規模擷取、上傳和儲存的支援。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">Assets HTTP API</a>：</strong>
            透過HTTP和JSON以程式設計方式與資產互動。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">連結共用</a>：</strong>
            無需登入即可輕鬆臨機共用數位資產。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html" target="_blank">Brand Portal</a>：</strong>
            雲端服務SAAS解決方案，可順暢共用及發佈數位資產。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">連線資產</a>：</strong>
            AEM Sites執行個體可順暢地存取和使用其他AEM Assets執行個體的資產。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">Asset Insights</a>：</strong>
            運用Adobe Analytics來擷取客戶與數位資產的互動，並在AEM中檢視。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">多語言資產</a>：</strong>
            使用語言根自動支援資產中繼資料。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">智慧標籤與管制</a>：</strong>
            運用Adobe Sensei自動使用有用的中繼資料標籤影像。</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">智慧型翻譯搜尋</a>：</strong>
            搜尋AEM Assets時自動翻譯搜尋字詞。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank">Adobe InDesign Server整合</a>：</strong>
            產生產品目錄。 根據InDesign範本製作手冊、傳單和印刷廣告。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/cn/experience-manager/desktop-app/aem-desktop-app.html" target="_blank">AEM案頭應用程式</a>：</strong>
            將資產同步至本機案頭，以便使用Creative Suite產品進行編輯。
            </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Adobe Imaging Library</a>：</strong>
                <br> 用於高品質檔案操控的Photoshop和AcrobatPDF資料庫。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/cn/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Adobe資產連結</a>：</strong>
            直接從Adobe建立雲端應用程式存取AEM Assets。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Adobe Stock整合</a>：</strong>
            直接從AEM無縫存取及使用Adobe Stock影像。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

### AEM Assets Dynamic Media

***✔<sup>+</sup> 此版本大幅增強功能。***

***✔<sup>SP</sup> 表示此功能可透過Service Pack或Feature Pack取得。***


<table>
    <thead>
        <tr>
            <td>Dynamic Media功能</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3 +FP</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">影像</a>：</strong>
            以不同大小和格式（包括智慧型裁切）動態傳送影像。</td>
            <td> </td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">視訊</a>：</strong>
            進階視訊編碼和自我調整視訊串流</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">互動媒體</a>：</strong>
            建立互動式橫幅、包含可點按內容的影片，以展示重要選件。
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>集合(<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">影像</a>， <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">迴轉</a>， <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">混合媒體</a>)：</strong>
            允許使用者縮放、平移、旋轉和模擬360度的檢視體驗。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/" target="_blank">檢視者</a>：</strong>
            自訂品牌豐富型媒體播放器和預設集，並支援不同熒幕/裝置。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">傳遞</a>：</strong>
            透過HTTP/2通訊協定連結或內嵌Dynamic Media內容及傳遞的彈性選項。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>從Scene7升級至Dynamic Media：</strong>
            可移轉主要資產並繼續使用現有S7 URL。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

## Forms功能

以下是AEM提供的重要AEM Forms附加元件功能對照表。 其中部分功能已在每個版本的舊版中新增的增量增強功能中引入。

***✔<sup>+</sup> 此版本大幅增強功能。***

***✔<sup>SP</sup> 表示此功能可透過Service Pack或Feature Pack取得。***

<table>
    <thead>
        <tr>
            <td>Forms功能</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">最適化Forms編輯器</a>：</strong>
            根據裝置和瀏覽器設定，建立引人入勝、回應式且最適化的表單。</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">記錄檔案</a>：</strong>
            建立檔案，以確保資料擷取體驗或可供列印版本長期儲存。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/themes.html" target="_blank">主題編輯器</a>：</strong>
            建立可重複使用的主題來設定表單元件和面板的樣式。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/template-editor.html" target="_blank">範本編輯器</a>：</strong>
            標準化及實作最適化表單的最佳作法。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Acrobat Sign整合</a>：</strong>
            允許部署以Acrobat Sign整合式表單為基礎的簽署情境。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">通訊管理</a>：</strong>
            透過AEM Forms，您可以建立、管理和提供個人化和互動式客戶信函。
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">協力廠商資料整合</a>：</strong>
            透過資料整合，系統會根據使用者在表單中的輸入，從不同的資料來源擷取資料。 在提交表單時，擷取的資料會回寫至資料來源。
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Forms處理的工作流程（在OSGi上）</a>：</strong>
            簡化表單核准流程的部署。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/user-guide.html?topic=/experience-manager/6-5/forms/morehelp/integrations.ug.js" target="_blank">與Marketing Cloud整合</a>：</strong>
            與Adobe Analytics和Adobe Target整合，以強化和測量客戶體驗。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">表單管理員</a>：</strong>
            管理所有表單/檔案/信函的單一位置，例如啟用分析、翻譯、A/B測試、稽核和發佈。
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">AEM Forms應用程式</a>：</strong>
            允許在iOS、Android或Windows上的應用程式內進行線上/離線表單處理。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">互動式通訊</a>：</strong>
            使用互動式元素(例如圖表（以前稱為Adaptive Documents）)建立豐富通訊（例如目標陳述式）。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Forms處理的工作流程(J2EE)：</strong>
            使用直覺式IDE建置複雜的表單/以檔案為中心的工作流程。</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">AEM Forms檔案安全性</a>：</strong>
            安全地存取和授權PDF和Office檔案。
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">測試架構</a>：</strong>
            使用Calvin框架和Chrome外掛程式來支援和偵錯調適型表單。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

## Communities功能

以下是AEM提供的重要AEM Communities附加元件功能對照表。 其中部分功能已在每個版本的舊版中新增的增量增強功能中引入。

***✔<sup>+</sup> 此版本大幅增強功能。***

***✔<sup>SP</sup> 表示此功能可透過Service Pack或Feature Pack取得。***

<table>
    <thead>
        <tr>
            <td> </td>
            <td>社群功能</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="7">社群功能</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/forum.html" target="_blank">論壇</a>：</strong> （社交元件架構）建立新主題，或檢視、關注、搜尋和移動現有主題。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <p><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-qna.html" target="_blank">QnA</a>：</strong>
                詢問、檢視和回答問題。</p>
            </td>
            <td></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/blog-feature.html" target="_blank">部落格</a>：</strong>
                在發佈端建立部落格和評論。
            </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/ideation-feature.html" target="_blank">構思</a>：</strong>
                建立並與社群分享想法，或檢視、關注並評論現有想法。
            </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/calendar.html" target="_blank">行事曆</a>：</strong>
                （社交元件架構）為網站訪客提供社群事件資訊。
            </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/file-library.html" target="_blank">檔案庫</a>：</strong>
                上傳、管理和下載社群網站內的檔案。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/users.html#AboutCommunityGroups" target="_blank">使用者群組</a>：
            </strong>一組使用者可以屬於成員群組，也可以被集體指派角色。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong> </strong></td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">指定任務</a>：</strong>
            建立並指派學習資源給社群成員。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">启用</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/catalog.html" target="_blank">目錄</a> 和 <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">資源管理</a>：</strong>
            從目錄存取啟用資源。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#CreateaLearningPath" target="_blank">學習路徑管理</a>：</strong>
            管理課程或啟用資源群組。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reports.html#main-pars_text_1739724213" target="_blank">啟用報告</a>：</strong>
            有關啟用資源和學習路徑的報告。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#main-pars_text_899882038" target="_blank">啟用時的參與</a>：</strong>
            新增啟用資源的註解。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">啟用Analytics</a>：</strong>
            視訊分析、進度報告和指派報告</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="8">Commons</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/comments.html" target="_blank">註解</a> 和附件：</strong>
            （社交元件架構）身為社群成員，可分享關於Communities網站內容的意見與知識。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>內容片段轉換：</strong>
            將UGC貢獻內容轉換為內容片段。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reviews.html" target="_blank">評論</a>：</strong>
                （社交元件架構）身為社群成員，可使用評論和評等功能組合來檢閱內容。</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/rating.html" target="_blank">評等</a>：/strong&gt; （社交元件架構）身為社群成員，對內容內容進行評分。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/voting.html" target="_blank">投票</a>：</strong>
                （社交元件架構）身為社群成員的某段內容可投贊成票或反對票。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tag-ugc.html" target="_blank">標籤</a>：</strong>
            將標籤（關鍵字或標籤）與內容附加以快速找到內容。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/search.html" target="_blank">搜尋</a>：</strong>
            預測性和建議性搜尋。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/translate-ugc.html" target="_blank">翻譯</a>：</strong>
            使用者產生內容的機器翻譯。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="10">管理</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/create-site.html" target="_blank">網站管理</a>：</strong>
            建立具有社群功能的網站。</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">範本</a>：</strong>
                <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">網站</a> 和 <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tools-groups.html" target="_blank">群組</a> 以精靈為基礎建立完整功能的社群網站的範本。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>可編輯的範本：</strong>
            讓社群管理員能夠使用AEM可編輯範本建置豐富的體驗。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/creating-groups.html" target="_blank">群組或子社群</a>：</strong>
            在社群網站內動態建立子社群。
            </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/in-context.html" target="_blank">稽核</a>：</strong>
            稽核使用者產生的內容。
            </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderation.html" target="_blank">大量稽核</a>：</strong>
            管理主控台，大量管理使用者產生的內容。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderate-ugc.html#CommonModerationConcepts" target="_blank">垃圾郵件偵測和髒話篩選器</a>：</strong>
            自動偵測垃圾郵件。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/members.html" target="_blank">成員管理</a>：</strong>
            從成員管理區域管理使用者設定檔和群組。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html#main-pars_text_866731966" target="_blank">回應式設計</a>：</strong>
            AEM Communities網站有回應。
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">分析</a>：</strong>
            與Adobe Analytics整合，以取得社群網站使用情形的重要深入分析。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="4">成员</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/advanced.html" target="_blank">評分和徽章</a>：</strong>
            (由Adobe Sensei提供支援的進階評分)將社群成員識別為專家並給予獎勵。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/activities.html" target="_blank">活動</a> 和 <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/notifications.html" target="_blank">通知</a>：</strong>
            檢視最近的活動資料流，並取得有關感興趣事件的通知。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/configure-messaging.html" target="_blank">訊息</a>：</strong>
            直接傳送訊息給使用者和群組。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/social-login.html" target="_blank">社交登入</a>：</strong>
            使用其Facebook或Twitter帳戶登入。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">Platform</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">MSRP （Mongo儲存）</a>：</strong>
            使用者產生的內容(UGC)會直接保留在本機MongoDB執行個體中</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">DSRP （資料庫儲存）</a>：</strong>
            使用者產生的內容(UGC)會直接保留在本機MySQL資料庫執行個體中。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">SRP （雲端儲存空間）</a>：</strong>
                使用者產生的內容(UGC)會從遠端儲存在由Adobe託管和管理的雲端服務中。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank"><strong>JSRP</a>：</strong>
                社群內容儲存在JCR中，UGC可從發佈該內容的author （或publish）執行個體存取。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sync.html" target="_blank">使用者和群組同步</a>：</strong>
            使用發佈伺服器陣列拓撲時，跨發佈執行個體同步化使用者和群組。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

AEM Communities透過發行版本新增增強功能，讓組織能夠透過以下方式與其使用者互動並提升其使用能力：

+ **@mention** 支援使用者產生的內容。
+ 改善協助工具，透過 **鍵盤導覽** 在 **啟用** 元件。
+ 已改善 **大量仲裁** 使用 **自訂篩選器**.
+ **可編輯的範本** 讓社群管理員能夠在AEM中建立豐富的社群體驗。
+ 使用者現在可以傳送 **大量直接訊息** 至群組的所有成員。
