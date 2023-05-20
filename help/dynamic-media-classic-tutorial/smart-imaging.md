---
title: 智能成像
description: Dynamic Media Classic中的智慧型影像可根據使用者端瀏覽器功能，自動最佳化影像格式和品質，進而增強影像傳送效能。 其做法是運用Adobe Sensei AI功能並使用現有的影像預設集。 進一步瞭解智慧型影像處理，以及如何透過更快的頁面載入來使用智慧型影像處理提供更佳的客戶體驗。
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring, renditions, images
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
exl-id: 678671c3-af25-4da1-bc14-cbc4cc19be8d
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '687'
ht-degree: 2%

---

# 智能成像 {#smart-imaging}

客戶在您網站或行動網站或應用程式上的體驗中，最重要的一個方面是頁面載入時間。 如果頁面載入時間過長，客戶通常會捨棄網站或應用程式。 影像構成了大部分的頁面載入時間。 Dynamic Media Classic中的智慧型影像可根據使用者端瀏覽器功能，自動最佳化影像格式和品質，進而增強影像傳送效能。 其做法是運用Adobe Sensei AI功能並使用現有的影像預設集。 智慧型影像處理可將影像大小減少30%或更多，進而加快頁面載入速度，並提供更佳的客戶體驗。

智慧型影像還能與Adobe提供的同級最佳優質服務充分整合，進一步提升效能。 此服務會尋找伺服器、網路和對等點之間的最佳網際網路路由，其延遲和/或封包遺失率比網際網路上的預設路由要低。

進一步瞭解 [智慧型影像](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html).

## 智慧型影像的優點

由於影像構成了頁面大部分的載入時間，因此智慧型影像的效能提升可能會對業務KPI產生深遠影響，例如更高的轉換率、網站逗留時間以及較低的網站跳出率。

![图像](assets/smart-imaging/smart-imaging-1.png)

## 智慧型影像如何運作

如先前所述，智慧型影像處理運用Adobe Sensei AI功能，並與現有的影像預設集搭配使用，自動將影像轉換為最佳的新一代影像格式，例如WebP，同時維持視覺逼真度。

進一步瞭解 [智慧型影像如何運作](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html#how-does-smart-imaging-work)，包括支援的影像格式（以及若未使用這些格式會發生什麼情況）等詳細資料，及其對使用中的現有影像預設集的影響。

## 智慧型影像的影響

您可能擔心您必須變更網站上的URL、影像預設集和程式碼，才能使用智慧型影像處理。 如果您符合使用智慧型影像的先決條件，而且您只處理支援的JPEG和PNG影像格式的影像，則不需要進行任何變更。

智慧型影像處理透過HTTP、HTTPS和HTTP/2傳送的影像。

>[!NOTE]
>
>移到智慧型影像可清除CDN上的快取。 CDN中的快取通常會在一或兩天內重新建立。

智慧型影像包含於您現有的Dynamic Media Classic授權中。 此功能不需額外付費。 若要妥善運用，您必須符合兩項需求：具備Adobe隨附的CDN和專用網域。 然後，您必須為您的帳戶啟用它，因為它未自動啟用。

若要啟用智慧型影像，您必須透過以下方式傳送技術支援請求： |建立支援案例| [https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html). 支援人員將與您合作，設定您將與智慧型影像建立關聯的自訂網域。 您將變更一個與快取相關的引數（存留時間或TTL），支援將清除快取。 您也可以選擇在推送至生產環境前執行選擇性預備步驟。 之後，當「智慧型影像」開啟時，您會以客戶要求的相同品質，提供尺寸較小的影像。 這表示它們的頁面載入速度更快 — 而且所有這一切都是自動完成的，因為Adobe Sensei有助於選擇最有效率的大小。

啟用智慧型影像之後，您需要確認它是否如預期般運作。

您可能有其他關於智慧型影像的問題。 我們已整理好常見問題集(FAQ)的清單，並提供解答。 閱讀 [常見問答](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html).

## 其他资源

觀看 [Dynamic Media Classic頁面效能最佳化技能培養](https://seminars.adobeconnect.com/pzc1gw0cihpv) 隨選網路研討會，進一步瞭解智慧影像處理。
