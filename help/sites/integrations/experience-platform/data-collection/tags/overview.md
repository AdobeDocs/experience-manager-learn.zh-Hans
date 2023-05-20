---
title: 整合Experience Platform資料收集標籤(Launch)和AEM
description: Experience Platform Data Collection中的標籤是Adobe的下一代標籤管理解決方案，也是部署Adobe Analytics、Target、Audience Manager和更多解決方案的最佳方式。 取得標籤（先前稱為Launch）的概觀，以及與Adobe Experience Manager整合的建議。
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 5979
thumbnail: 39090.jpg
topic: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-07-10T00:00:00Z
exl-id: bdae56d8-96e7-4b05-9b8b-3c6c2e998bd8
source-git-commit: 1d2daf53cd28fcd35cb2ea5c50e29b447790917a
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 2%

---

# 整合Experience Platform資料收集標籤和AEM {#overview}

瞭解如何整合Experience Platform _資料收集標籤_ （先前稱為Launch）與Adobe Experience Manager。

>[!NOTE]
>
>Adobe Experience Platform Launch已經過品牌重塑，現在是Adobe Experience Platform中的一套資料收集技術。 因此，產品檔案中出現了幾項術語變更。 請參閱下列內容 [檔案](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html) 以取得術語變更的彙整參考資料。


標籤是Adobe Experience Platform的下一代標籤管理技術。 標籤提供部署Adobe Analytics、Target、Audience Manager和更多解決方案的最簡單方式。 取得標籤概觀，並取得建議的Adobe Experience Manager整合。

>[!VIDEO](https://video.tv.adobe.com/v/3417061?quality=12&learn=on)


## 前提条件

整合Experience Platform資料收集標籤時，需要下列專案。

+ AEM管理員對AEMas a Cloud Service環境的存取權
+ 引用網站，例如 [WKND](https://github.com/adobe/aem-guides-wknd) 已部署至其中。
+ 存取Adobe Experience Platform資料收集解決方案
+ 系統管理員的存取權 [Adobe Developer主控台](https://developer.adobe.com/developer-console/)


## 高階步驟

+ 在Adobe Experience Platform資料彙集中建立Tag屬性並編輯為 _新增規則_. 則 _新增程式庫_，選取新新增的規則、核准並發佈。
+ 使用現有（或新的） IMS設定連線AEM和標籤
+ 在AEM中，建立Launch雲端服務設定，然後將其套用至現有網站，最後確認Tags屬性及其程式庫已載入已發佈或作者網站。

## 后续步骤

[建立標籤屬性](create-tag-property.md)

## 其他资源 {#additional-resources}

+ [Experience Platform與Experience Cloud應用程式的整合](https://experienceleague.adobe.com/docs/platform-learn/tutorials/intro-to-platform/integrations-with-experience-cloud-applications.html)
+ [標籤總覽](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html)
+ [在具有標籤的網站中實作Experience Cloud](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/overview.html)
