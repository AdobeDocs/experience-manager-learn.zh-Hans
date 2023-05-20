---
title: 在AEM Sites中擴充頁面屬性
description: 瞭解如何延伸Adobe Experience Manager Sites中頁面屬性的中繼資料欄位。 本影片詳細介紹使用Sling資源合併功能最有效達成此目標的方式。
topic: Development
feature: Core Components
role: Developer
version: Cloud Service
kt: 243
thumbnail: 25173.jpg
exl-id: 500f4e07-2686-42a2-8e44-d96dde02a112
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 1%

---

# 擴充頁面屬性 {#extending-page-properties-in-aem-sites}

自訂「頁面屬性」的中繼資料欄位是任何Sites實作中的常見要求。 本影片詳細介紹使用Sling資源合併功能最有效達成此目標的方式。

>[!VIDEO](https://video.tv.adobe.com/v/25173?quality=12&learn=on)

上述影片顯示自訂的頁面屬性 [WKND參考網站](https://github.com/adobe/aem-guides-wknd).

## 範例WKND頁面屬性套件

您可以使用提供的 [範例WKND頁面屬性套件](./assets/WKND-PageProperties-Example-Dialog-1.0.zip) 包含 **WKND** 和 **基本** 以上影片中顯示的標籤自訂。 此 **社群媒體** 索引標籤自訂未提供為 [WKND頁面元件](https://github.com/adobe/aem-guides-wknd/blob/main/ui.apps/src/main/content/jcr_root/apps/wknd/components/page/.content.xml#L5) 現在使用V3版本的WCM核心元件，而在V3版本中 [已棄用社交分享](https://github.com/adobe/aem-core-wcm-components/pull/1930).

出於學習目的，您可以使用將WKND頁面元件指向V2版的WCM核心元件 `sling:resourceSuperType` 屬性值並覆蓋 [社群媒體](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/page/v2/page/_cq_dialog/.content.xml#L95) 標籤。 如需詳細資訊，請參閱 [設定頁面屬性](https://experienceleague.adobe.com/docs/experience-manager-65/developing/extending-aem/page-properties-views.html#configuring-your-page-properties)

本範例套件應安裝在本機AEM SDK或AEM 6.X.X執行個體上，以供學習之用。
