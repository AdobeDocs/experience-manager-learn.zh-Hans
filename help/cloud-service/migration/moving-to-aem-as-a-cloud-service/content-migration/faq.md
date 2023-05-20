---
title: AEMas a Cloud Service內容移轉常見問題集
description: 取得有關將內容移轉至AEMas a Cloud Service的常見問題解答。
version: Cloud Service
doc-type: article
feature: Migration
topic: Migration
role: Architect, Developer
level: Beginner
kt: 11200
thumbnail: kt-11200.jpg
exl-id: bdec6cb0-34a0-4a28-b580-4d8f6a249d01
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '2283'
ht-degree: 1%

---

# AEMas a Cloud Service內容移轉常見問題集

取得有關將內容移轉至AEMas a Cloud Service的常見問題解答。

## 术语

+ **AemCS**： [AEMas a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/introduction.html)
+ **BPA**： [Best Practices Analyzer](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/best-practices-analyzer/overview-best-practices-analyzer.html)
+ **CTT**： [內容轉移工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html)
+ **凸輪**： [Cloud Acceleration Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/getting-started-cam.html)
+ **IMS**： [Identity Management系統](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html)
+ **DM**： [Dynamic Media](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/dynamicmedia/dm-journey/dm-journey-part1.html)

建立CTT相關Adobe支援票證時，請使用以下範本提供詳細資訊。

![內容移轉Adobe支援票證範本](../../assets/faq/adobe-support-ticket-template.png) { align=&quot;center&quot; }

## 一般內容移轉問題

### 問：將內容移轉至AEM做為Cloud Services有哪些不同方法？

有三種不同的方法可供使用

+ 使用內容轉移工具(AEM 6.3+ → AEMaaCS)
+ 透過封裝管理員(AEM→AEMaaCS)
+ 立即可用的資產(S3/Azure→AEMaaCS)大量匯入服務

### 問：可以使用CTT傳輸的內容數量是否有限制？

否. CTT工具可從AEM來源擷取並擷取至AEMaaCS。 不過，在移轉之前，應該考量AEMaaCS平台的特定限制。

如需詳細資訊，請參閱 [雲端移轉必要條件](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/prerequisites-content-transfer-tool.html).

### 問：我有來自來源系統的最新BPA報告，應該如何處理它？

將報表匯出為CSV，然後將其上傳到Cloud Acceleration Manager， [與您的IMS組織相關聯](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-acceleration-manager/using-cam/getting-started-cam.html). 然後以下列方式完成稽核程式 [整備階段中概述](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-acceleration-manager/using-cam/cam-readiness-phase.html).

請檢閱工具提供的程式碼和內容複雜性評估，並記下導致程式碼重構待辦或雲端移轉評估的相關行動專案。

### 問：建議您在來源作者上擷取，然後內嵌至AEMaaCS作者和發佈嗎？

在製作和發佈層級之間，建議一律執行1:1擷取和內嵌。 也就是說，擷取來源生產作者並將其擷取到開發、測試和生產CS中是可接受的作法。

### 問：是否有方法可以估計使用CTT將內容從來源AEM移轉至AEMaaCS所需的時間？

由於移轉程式取決於網際網路頻寬、配置給CTT程式的棧積、可用的記憶體以及磁碟IO （對每個來源系統而言都是主觀的），因此建議及早執行移轉證明，並外推該資料點得出估計值。

### 問：如果我啟動CTT擷取程式，來源AEM效能會受到什麼影響？

CTT工具會在自己的Java™流程中執行，最多需要4gb棧積，可透過OSGi設定設定。 此數字可能會變更，但您可以搜尋Java™流程並找出原因。

如果已安裝AZCopy且/或已啟用預先複製選項/驗證功能，則AZCopy處理序會消耗CPU週期。

除了jvm之外，此工具也會使用磁碟IO將資料儲存在可轉換的暫存空間中，而且這些資料將在擷取週期後清除。 除了RAM、CPU和磁碟IO之外，CTT工具也使用來源系統的網路頻寬將資料上傳到Azure blob存放區。

CTT擷取流程所需的資源量取決於節點數量、Blob數量及其彙總大小。 很難提供公式，因此建議執行小型移轉證明，以判斷來源伺服器升級需求。

如果使用翻制環境進行移轉，則不會影響即時生產伺服器資源使用率，但在即時生產與翻制之間同步內容方面，有其本身的缺點

### 問：在我的來源作者系統中，我們已設定SSO讓使用者透過驗證進入作者執行個體。 在此情況下，我是否必須使用CTT的使用者對應功能？

簡短的答案是&quot;**是**「。

CTT擷取和擷取 **不含** 使用者對應只會將內容、關聯原則（使用者、群組）從來源AEM移轉到AEMaaCS。 但是，這些使用者（身分）需要存在於Adobe IMS中並（布建）有AEMaaCS執行個體的存取權，才能成功驗證。 工作： [使用者對應工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/user-mapping-tool/overview-user-mapping-tool.html) 是將本機AEM使用者對應至IMS使用者，以便驗證和授權共同運作。

在此情況下，SAML身分提供者會針對Adobe IMS設定為使用同盟/Enterprise ID，而不是使用驗證處理常式直接設定為AEM。

### 問：在我的來源作者系統中，我們已設定基本驗證，讓使用者透過本機AEM使用者驗證到作者執行個體中。 在此情況下，我是否必須使用CTT的使用者對應功能？

簡短的答案是&quot;**是**「。

沒有使用者對應的CTT擷取和內嵌確實將內容、相關原則（使用者、群組）從來源AEM移轉到AEMaaCS。 但是，這些使用者（身分）需要存在於Adobe IMS中並（布建）有AEMaaCS執行個體的存取權，才能成功驗證。 工作： [使用者對應工具](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/user-mapping-tool/overview-user-mapping-tool.html) 是將本機AEM使用者對應至IMS使用者，以便驗證和授權共同運作。

在此情況下，使用者會使用個人Adobe ID，而IMS管理員會使用Adobe ID來提供AEMaaCS的存取權。

### 問：在CTT的情境下，「擦去」和「覆寫」這兩個辭彙是什麼意思？

在的內容中 [擷取階段](https://experienceleague.adobe.com/docs/experience-manager-cloud-servicemoving/cloud-migration/content-transfer-tool/extracting-content.html)，選項包括覆寫中繼容器中先前擷取循環的資料，或新增差異（新增/更新/刪除）至其中。 暫存容器只是與移轉集相關聯的blob儲存容器，什麼都不是。 每個移轉集都有各自的暫存容器。

在的內容中 [擷取階段](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/ingesting-content.html)，選項為+可取代AEMaaCS的整個內容存放庫，或是從中繼移轉容器同步差異（新增/更新/刪除）內容。

### 問：來源系統中有多個網站、相關資產、使用者和群組。 是否可以分階段將其移轉至AEMaaCS？

可以，但需要謹慎規劃：

+ 建立移轉集時會假設網站和資產位於其各自的階層中
   + 確認是否可接受在一個移轉集中移轉所有資產，然後分階段帶入使用這些資產的網站
+ 在目前狀態，作者擷取程式會使得作者例項無法用於內容製作，即使發佈層級仍可提供內容
   + 這表示在擷取完成到作者中之前，內容製作活動會凍結

在規劃移轉之前，請先檢閱追加擷取和擷取程式，如檔案所述。

### 問：即使AEMaaCS作者或發佈執行個體中發生內嵌，我的網站是否仍可供一般使用者使用？

是。內容移轉活動不會中斷一般使用者流量。 不過，作者擷取會凍結內容製作，直到完成為止。

### 問：BPA報表會顯示與遺失原始轉譯相關的專案。 我是否應在擷取前清理來源上的這些專案？

是。缺少原始轉譯表示資產二進位檔一開始未正確上傳。 將其視為不良資料，請檢閱，使用封裝管理員（視需要）進行備份，並在執行擷取之前從來源AEM中將其移除。 不良資料會在資產處理步驟上產生負面結果。

### 問：BPA報表含有與遺失相關的專案 `jcr:content` 資料夾的節點。 我該如何處理它們？

時間 `jcr:content` 資料夾層級遺失任何可傳播設定的動作，例如處理設定檔等。 來自父級將在此層級中斷。 請檢視遺失的原因 `jcr:content`. 雖然這些資料夾可以移轉，但請注意，這類資料夾會降低使用者體驗，並造成後續不必要的疑難排解週期。

### 問：我已經建立了移轉集。 是否可以檢查其大小？

是，有一個 [檢查大小](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html#migration-set-size) 屬於CTT一部分的特徵。

### 問：我正在執行移轉（擷取、擷取）。 是否可以驗證我擷取的所有內容是否都已擷取到目標中？

是，有一個 [驗證](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/validating-content-transfers.html) 屬於CTT之一部分的功能。

### 問：我的客戶需要在AEMaaCS環境之間移動內容，例如從AEMaaCS Dev到AEMaaCS Stage或到AEMaaCS Prod。 我可以針對這些使用案例使用內容轉移工具嗎？

很遺憾，否。 CTT的使用案例是將內容從內部部署/AMS託管的AEM 6.3+來源移轉到AEMaaCS雲端環境。 [請閱讀CTT檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html).

### 問：在擷取期間會預期出現哪些問題？

提取階段是一個涉及的過程，需要多個方面才能如預期般運作。 瞭解可能會發生的各種問題以及如何緩解這些問題，可提高內容移轉的整體成功率。

公開檔案會根據學習內容持續改善，以下是一些高階問題類別和可能的潛在原因。

![AEMas a Cloud Service內容移轉擷取問題](../../assets/faq/extraction-issues.jpg) { align=&quot;center&quot; }

### 問：在擷取期間可能會出現哪些問題？

擷取階段會完全在雲端平台中進行，並且需要有權存取AEMaaCS基礎結構的資源的協助。 請建立支援票證以取得更多說明。

以下是可能的問題類別（請勿將此視為獨佔清單）

![AEMas a Cloud Service內容移轉擷取問題](../../assets/faq/ingestion-issues.jpg) { align=&quot;center&quot; }



### 問：我的來源伺服器需要輸出網際網路連線，CTT才能運作嗎？

簡短的答案是&quot;**是**「。

CTT程式需要連線至下列資源：

+ 目標AEMas a Cloud Service環境： `author-p<program_id>-e<env_id>.adobeaemcloud.com`
+ Azure Blob儲存服務： `casstorageprod.blob.core.windows.net`
+ 使用者對應IO端點： `usermanagement.adobe.io`

請參閱檔案以取得更多關於 [來源連線能力](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html#source-environment-connectivity).

## 資產處理Dynamic Media相關問題

### 問：資產在AEMaaCS中擷取後是否會自動重新處理？

否. 若要處理資產，必須起始重新處理的請求。

### 問：資產在AEMaaCS中擷取後是否會自動重新索引？

是。系統會根據AEMaaCS上可用的索引定義，將資產重新編列索引。

### 問：來源AEM與Dynamic Media整合。 在內容移轉之前，是否有任何必須考量的特定事項？

是，當來源AEM具有Dynamic Media整合時，請考慮下列事項。

+ AEMaaCS僅支援Dynamic Media Scene7模式。 如果來源系統處於混合模式，則需要將DM移轉至Scene7模式。
+ 如果方法是從來源複製執行個體移轉，則可在用於CTT的複製上停用DM整合是安全的。 此步驟純粹是為了避免寫入DM或避免DM流量載入。
+ 請注意，CTT會將節點、移轉集的中繼資料從來源AEM移轉到AEMaaCS。 它不會直接在DM上執行任何作業。

### 問：當來源AEM上出現DM整合時，有哪些不同的移轉方法？

請先閱讀上述問題和答案

（這兩個選項皆可，但不限於這二個）。 這取決於客戶希望如何進行UAT、效能測試、可用的環境，以及是否使用翻制進行移轉。 請將這兩者視為討論的起點

**選項1**

如果來源環境中的資產/節點數量位於低端（約100K），假設這些資產可於24 + 72小時的期間內移轉（包括提取和擷取），則更好的方法為

+ 直接從生產環境執行移轉
+ 使用執行初始擷取和內嵌到AEMaaCS `wipe=true`
   + 此步驟會移轉所有節點和二進位檔
+ 繼續使用內部部署/AMS Prod作者
+ 從現在開始，請透過以下執行所有其他移轉週期證明 `wipe=true`
   + 請注意，此操作會移轉完整節點存放區，但只移轉修改過的Blob而非整個Blob。 前一組的Blob位於目標AEMaaCS執行個體的Azure Blob存放區中。
   + 使用此移轉證明來測量移轉持續時間、使用者對應、測試、驗證所有其他功能
+ 最後，在上線當週之前，執行擦去=true移轉
   + 在AEMaaCS上連線Dynamic Media
   + 中斷與AEM內部部署來源的DM設定

使用此選項，您可以執行一對一移轉，也就是內部部署→AEMaaCS Dev等。 並從個別環境移動DM設定

（如果移轉計畫從複製執行）

**選項2**

+ 建立生產作者的翻制，從翻制中移除DM設定
+ 將內部部署複製移→AEMaaCS Dev/Stage
   + 將生產DM公司短暫連線到AEMaaCS Dev/Stage以進行驗證
   + 在DM連線作用中期間，請避免將資產擷取到AEMaaCS
   + 這可讓他們驗證CTT、DM特定驗證
+ 在AEMaaCS上完成測試後
   + 執行從內部部署階段到AEMaaCS階段的抹除移轉

執行從內部部署開發到AEMaaCS開發的抹除移轉。

上述方法僅可用於測量移轉期間，但需要稍後加以清理。

## 其他資源

+ [在雲端中移轉至Experience Manager的秘訣與技巧( Summit 2022)](https://business.adobe.com/summit/2022/sessions/tips-and-tricks-for-migrating-to-experience-manage-tw109.html)

+ [CTT專家系列影片](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-servicemigration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.html)

+ [其他AEMaaCS主題的專家系列影片](https://experienceleague.adobe.com/docs/experience-manager-learncloud-service/aem-experts-series.html)
