---
title: 使用Experience Platform Launch和Adobe I/O整合Adobe Experience Manager與Adobe Target
seo-title: Integrating Adobe Experience Manager with Adobe Target using Experience Platform Launch and Adobe I/O
description: 逐步說明如何使用Experience Platform Launch和Adobe I/O將Adobe Experience Manager與Adobe Target整合
seo-description: Step by step walk-through on how to integrate Adobe Experience Manager with Adobe Target using Experience Platform Launch and Adobe I/O
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: b1d7ce04-0127-4539-a5e1-802d7b9427dd
source-git-commit: 4b47daf82e27f6bea4be30e3cdd132f497f4c609
workflow-type: tm+mt
source-wordcount: '1064'
ht-degree: 4%

---

# 透過Adobe I/O控制檯使用Adobe Experience Platform Launch

## 前提条件

* [AEM作者和發佈執行個體](./implementation.md#set-up-aem) 分別在localhost連線埠4502和4503上執行
* **Experience Cloud**
   * 存取您的組織Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * 布建了下列解決方案的Experience Cloud
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/O主控台](https://console.adobe.io)

      >[!NOTE]
      >您應具有在Launch中開發、核准、發佈、管理擴充功能及管理環境的許可權。 如果您因無法使用的使用者介面選項而無法完成其中任何步驟，請聯絡您的Experience Cloud管理員以請求存取權。 如需Launch許可權的詳細資訊， [請參閱檔案](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/user-permissions.html).


* **瀏覽器外掛程式**
   * Adobe Experience Cloud Debugger ([鉻黃](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj))
   * Launch與DTM交換器([鉻黃](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk))

## 相關使用者

進行這項整合時，需要涉及下列對象，而若要執行某些工作，您可能需要管理存取權。

* 开发人员
* AEM管理員
* Experience Cloud管理員

## 简介

AEM 提供与 Experience Platform Launch 的现成集成。此整合可讓AEM管理員透過易於使用的介面輕鬆設定Experience Platform Launch，進而在設定這兩個工具時減少工作量和錯誤次數。 此外，只要將Adobe Target擴充功能新增至Experience Platform Launch，就能協助我們在AEM網頁上使用Adobe Target的所有功能。

在本節中，我們將介紹以下整合步驟：

* 启动
   * 创建 Launch 属性
   * 新增Target擴充功能
   * 建立資料元素
   * 创建页面规则
   * 設定環境
   * 构建和发布
* AEM
   * 建立Cloud Service
   * 创建

### 启动

#### 创建 Launch 属性

屬性是一個容器，當您將標籤部署至網站時，在其中裝入擴充功能、規則、資料元素和程式庫。

1. 導覽至您的組織 [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (`https://<yourcompany>.experiencecloud.adobe.com`)
2. 使用您的Adobe ID登入，並確認您隸屬於正確的組織。
3. 在解決方案切換器中，按一下 **Launch** 然後選取 **前往Launch** 按鈕。

   ![Experience Cloud — 啟動](assets/using-launch-adobe-io/exc-cloud-launch.png)

4. 請確認您隸屬於正確的組織，然後繼續建立Launch屬性。
   ![Experience Cloud — 啟動](assets/using-launch-adobe-io/launch-create-property.png)

   *如需建立屬性的詳細資訊，請參閱 [建立屬性](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/companies-and-properties.html?lang=en#create-or-configure-a-property) 產品檔案內。*
5. 按一下 **新增屬性** 按鈕
6. 提供屬性的名稱(例如， *AEM Target教學*)
7. 網域請輸入 *localhost.com* 因為這是執行WKND示範網站的網域。 雖然「*網域*「欄位」為必填欄位，則Launch屬性可在實施該屬性的任何網域上運作。 此欄位的主要用途是在規則產生器中預先填入功能表選項。
8. 按一下 **儲存** 按鈕。

   ![Launch — 新屬性](assets/using-launch-adobe-io/exc-launch-property.png)

9. 開啟您剛建立的屬性，然後按一下「擴充功能」標籤。

#### 新增Target擴充功能

Adobe Target擴充功能支援將Target JavaScript SDK用於現代網路的使用者端實作。 `at.js`. 仍在使用Target舊程式庫的客戶， `mbox.js`， [應升級至at.js](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/upgrading-from-atjs-1x-to-atjs-20.html) 以使用Launch。

Target擴充功能包含兩個主要部分：

* 管理核心程式庫設定的擴充功能設定
* 規則動作，可執行下列操作：
   * 載入Target (at.js)
   * 新增引數至所有Mbox
   * 將引數新增至全域mbox
   * 引發全域mbox

1. 下 **擴充功能**，即可檢視已針對Launch屬性安裝的擴充功能清單。 ([Experience Platform Launch核心擴充功能](https://exchange.adobe.com/experiencecloud.details.100223.adobe-launch-core-extension.html) 預設為安裝)
2. 按一下 **擴充功能目錄** 選項，並在篩選器中搜尋Target。
3. 選取最新版Adobe Target at.js並按一下 **安裝** 選項。
   ![Launch — 新增屬性](assets/using-launch-adobe-io/launch-target-extension.png)

4. 按一下 **設定** 按鈕，此時您可以注意到已匯入Target帳戶憑證的設定視窗，以及此擴充功能的at.js版本。
   ![Target — 擴充功能設定](assets/using-launch-adobe-io/launch-target-extension-2.png)

   透過非同步Launch內嵌程式碼部署Target時，您應在頁面上於Launch內嵌程式碼之前對預先隱藏的程式碼片段進行硬式編碼，以便管理內容閃爍問題。 我們稍後會進一步瞭解預先隱藏的Snipper。 您可以下載預先隱藏的程式碼片段 [此處](assets/using-launch-adobe-io/prehiding.js)

5. 按一下 **儲存** 若要完成將Target擴充功能新增至Launch屬性，您現在應該能夠看到Target擴充功能列於 **已安裝** 擴充功能清單。

6. 重複上述步驟以搜尋「Experience CloudID服務」擴充功能並加以安裝。
   ![擴充功能 — Experience CloudID服務](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### 設定環境

1. 按一下 **環境** 標籤中，即可檢視針對網站屬性建立的環境清單。 依預設，我們針對開發、測試和生產各建立一個例項。

![資料元素 — 頁面名稱](assets/using-launch-adobe-io/launch-environment-setup.png)

#### 构建和发布

1. 按一下 **發佈** 標籤中，接著建立程式庫，以建置變更（資料元素、規則）並將其部署至開發環境。
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. 將您從開發環境所做的變更發佈到測試環境。
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. 執行 **為測試環境建置選項**.
4. 建置完成後，請執行 **核准以發佈**，可將您的變更從測試環境移至生產環境。
   ![暫存至生產](assets/using-launch-adobe-io/build-staging.png)
5. 最後，執行 **建置並發佈至生產環境** 將變更推送至生產環境的選項。
   ![建置並發佈至生產環境](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> 授予Adobe I/O整合使用適當專案選取工作區的存取權 [角色，允許中央團隊僅在少數幾個工作區中進行API導向的變更](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html).

1. 使用來自Adobe I/O的憑證在AEM中建立IMS整合。（01:12到03:55）
2. 在Experience Platform Launch中建立屬性。 (涵蓋 [以上](#create-launch-property))
3. 使用步驟1中的IMS整合，建立Experience Platform Launch整合以匯入您的Launch屬性。
4. 在AEM中，使用瀏覽器設定將Experience Platform Launch整合對應至網站。 （05:28至06:14）
5. 手動驗證整合。 （06:15至06:33）
6. 使用Launch/DTM瀏覽器外掛程式。 （06:34至06:50）
7. 使用Adobe Experience Cloud Debugger瀏覽器外掛程式 （06:51至07:22）

此時，您已成功整合 [使用Adobe Experience Platform Launch的AEM與Adobe Target](./using-aem-cloud-services.md#integrating-aem-target-options) 如選項1所詳述。

若要使用AEM體驗片段選件來強化您的個人化活動，請前往下一章，並使用舊版雲端服務整合AEM與Adobe Target。
