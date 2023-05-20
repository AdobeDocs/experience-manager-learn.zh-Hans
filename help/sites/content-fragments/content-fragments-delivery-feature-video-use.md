---
title: 在AEM中傳送內容片段
seo-title: Delivering Content Fragments in Adobe Experience Manager
description: 內容片段與版面配置無關，可直接在搭配核心元件的AEM Sites中使用，或以Headless方式傳送至下游管道。
seo-description: Content Fragments, independent of layout, can be used directly in AEM Sites with Core Components or can be delivered in a headless manner to downstream channels.
feature: Content Fragments
topics: authoring, content-architecture
audience: all
doc-type: feature video
activity: use
version: 6.4, 6.5
uuid: 045473d2-5abe-4414-b91c-d369f3069ead
discoiquuid: 912e0c41-83cf-49f7-b515-09519b6718c1
topic: Content Management
role: User
level: Beginner
exl-id: 525cd30c-05bf-4f17-b61b-90609ce757ea
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 7%

---

# 傳送內容片段 {#delivering-content-fragments}

Adobe Experience Manager (AEM)內容片段是文字型編輯內容，其中可能包含一些相關聯但被視為純粹內容（沒有設計或版面配置資訊）的結構化資料元素。 內容片段通常建立為與管道無關的內容，其用途為跨管道使用和重複使用，這反過來又會在內容特定的體驗中包裝內容。

內容片段與版面配置無關，可直接在搭配核心元件的AEM Sites中使用，或以Headless方式傳送至下游管道。

本影片系列涵蓋使用內容片段的傳送選項。 有關定義與的詳細資訊 [您可以在此處找到編寫內容片段](content-fragments-feature-video-use.md).

1. 在網頁上使用內容片段
2. 使用AEM Content Services將內容片段公開為JSON
3. 使用Assets HTTP API

## 在網頁中使用內容片段 {#using-content-fragments-in-web-pages}

>[!VIDEO](https://video.tv.adobe.com/v/22449?quality=12&learn=on)

內容片段可以在AEM Sites頁面上使用，或是透過類似方式，使用AEM WCM Core Components的「 [內容片段元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=zh-Hans).

可以使用AEM樣式系統來設定內容片段元件的樣式，以視需要顯示內容。

## 以JSON格式公開內容片段 {#exposing-content-fragments-as-json}

>[!VIDEO](https://video.tv.adobe.com/v/22448?quality=12&learn=on)

AEM Content Services可協助建立AEM頁面型HTTP端點，將轉譯內容轉譯為標準化的JSON格式。

上述影片使用 [內容片段元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=zh-Hans) 以公開個別內容片段。 此 [內容片段清單元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-list.html) 是新元件，可讓作者定義查詢，以動態方式將內容片段清單填入頁面。 當需要公開多個內容片段時，建議使用內容片段清單元件。

*Content Services端點JSON裝載範例：*\
**[spitors.json](assets/athletes.json)**

## 使用Assets HTTP API

>[!VIDEO](https://video.tv.adobe.com/v/26390?quality=12&learn=on)

首次在AEM 6.5中推出，透過Assets HTTP API增強對內容片段的支援。 開發人員可透過此簡單方法，針對內容片段執行建立、讀取、更新和刪除(CRUD)操作。

*Postman請求範例：*
**[CRUD-CFM-API-We.Retail.postman_collection.json](assets/CRUD-CFM-API-We.Retail.postman_collection.json)**

## 要使用的傳送方法

### Web 渠道

透過Web頻道傳遞內容片段的方法直接透過將內容片段元件與AEM Sites搭配使用。

### Headless

在Headless使用案例中，有兩個選項可將內容片段公開為JSON以支援第三方管道：

1. 主要使用案例是交付內容片段供第三方管道使用（唯讀）時，請使用AEM內容服務和Proxy API頁面(影片#2本)。 Content Services架構針對哪些資料會公開提供更大的彈性和選項。 開發人員也可以擴充內容服務架構，以擴充及/或豐富資料。

2. 當第三方管道需要修改和/或更新內容片段時，請使用Assets HTTP API (影片#3)。 典型的使用案例是在AEM作者環境中擷取第三方內容。

## 其他资源 {#additional-resources}

* [创作内容片段](content-fragments-feature-video-use.md)
* [AEM WCM 核心组件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=zh-Hans)
* [AEM WCM核心內容片段元件](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=zh-Hans)

若要從影片系列下載並安裝以下套件至最終狀態的AEM 6.4+執行個體：\
**[aem_demo_fluid-experiencescontent-fragments-100.zip](assets/aem_demo_fluid-experiencescontent-fragments-100.zip)**
