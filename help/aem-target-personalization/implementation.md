---
title: 將Adobe Experience Manager與Adobe Target整合
seo-title: An article covering different ways to integrate Adobe Experience Manager(AEM) with Adobe Target for delivering personalized content.
description: 本文章說明如何針對不同案例使用Adobe Target設定Adobe Experience Manager。
seo-description: An article covering how to set up Adobe Experience Manager with Adobe Target for different scenarios.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: 54a30cd9-d94a-4de5-82a1-69ab2263980d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '659'
ht-degree: 3%

---

# 將Adobe Experience Manager與Adobe Target整合

在本節中，我們將討論如何針對不同案例使用Adobe Target設定Adobe Experience Manager。 根據您的案例和組織需求。

* **新增Adobe Target JavaScript程式庫（所有案例都需要）**
對於在AEM上託管的網站，您可以使用將Target程式庫新增至您的網站， [Launch](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html). Launch可讓您透過簡單的方式部署及管理所有必要的標籤，以便支援相關客戶體驗。
* **新增Adobe TargetCloud Services（體驗片段案例所需）**
對於想要使用體驗片段選件在Adobe Target中建立活動的AEM客戶，您需要使用舊版Cloud Services將Adobe Target與AEM整合。 從AEM以HTML/JSON選件的形式將體驗片段推送到Target，以及保持選件與AEM同步時，需要這項整合。 
*實作案例1需要此整合。*

## 前提条件

* **Adobe Experience Manager (AEM){#aem}**
   * AEM 6.5 (*建議使用最新的Service Pack*)
   * 下載AEM WKND參考網站套件
      * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [核心组件](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [數位資料層](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * 存取您的組織Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * 布建了下列解決方案的Experience Cloud
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/O主控台](https://console.adobe.io)

* **环境**
   * Java 1.8或Java 11 (僅限AEM 6.5+)
   * Apache Maven （3.3.9或更新版本）
   * 鉻黃

>[!NOTE]
>
> 客戶需要布建以下Experience Platform Launch和Adobe I/O： [Adobe支援](https://helpx.adobe.com/cn/contact/enterprise-support.ec.html) 或聯絡您的系統管理員

### 設定AEM{#set-up-aem}

完成本教學課程需要AEM作者和發佈執行個體。 我們已在執行作者執行個體 `http://localhost:4502` 和在上執行的發佈執行個體 `http://localhost:4503`. 如需詳細資訊，請參閱： [設定本機AEM開發環境](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html).

#### 設定AEM作者和發佈執行個體

1. 取得 [AEM Quickstart Jar和授權。](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. 在電腦上建立檔案夾結構，如下所示：
   ![文件夹结构](assets/implementation/aem-setup-1.png)
3. 將快速入門jar重新命名為 `aem-author-p4502.jar` 並將其放在 `/author` 目錄。 新增 `license.properties` 檔案於 `/author` 目錄。
   ![AEM作者例項](assets/implementation/aem-setup-author.png)
4. 複製Quickstart jar，將其重新命名為 `aem-publish-p4503.jar` 並將其放在 `/publish` 目錄。 新增 `license.properties` 檔案於 `/publish` 目錄。
   ![AEM發佈執行個體](assets/implementation/aem-setup-publish.png)
5. 按兩下 `aem-author-p4502.jar` 檔案安裝Author例項。 這會啟動編寫執行個體，在本機電腦的連線埠4502上執行。
6. 使用下列憑證登入，系統會在您成功登入後，將您導向至AEM首頁畫面。
使用者名稱： **管理員**
密碼： **管理員**
   ![AEM發佈執行個體](assets/implementation/aem-author-home-page.png)
7. 按兩下 `aem-publish-p4503.jar` 檔案以安裝發佈執行個體。 您會發現瀏覽器中針對您的發佈執行個體開啟了一個新索引標籤，在連線埠4503上執行並顯示WeRetail首頁。 我們在本教學課程中使用WKND參考網站，並在作者執行個體上安裝套件。
8. 在網頁瀏覽器中導覽至AEM作者，網址為 `http://localhost:4502`. 在AEM開始畫面上，導覽至 *[「工具>部署>套件」](http://localhost:4502/crx/packmgr/index.jsp)*.
9. 下載並上傳AEM的套件(列於上方的 *[先決條件> AEM](#aem)*)
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. 在AEM作者上安裝套件後，請在AEM套件管理器中選取每個上傳的套件，然後選取 **更多>復寫** 以確保套件部署至AEM Publish。
11. 此時，您已成功安裝WKND參考網站及本教學課程所需的所有其他套件。

[下一章節](./using-launch-adobe-io.md)：在下一章中，您將整合Launch與AEM。
