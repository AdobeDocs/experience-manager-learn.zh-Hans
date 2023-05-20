---
title: 搭配使用Smart Translation Search與AEM Assets
description: Smart Translation Search可跨AEM內容（資產和頁面）自動進行跨語言搜尋和探索，支援超過50種語言，並減少手動內容翻譯的需求。
version: 6.4, 6.5
feature: Search
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
thumbnail: 21297.jpg
exl-id: 4f35e3f7-ae29-4f93-bba9-48c60b800238
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 0%

---

# 搭配使用Smart Translation Search與AEM Assets{#using-smart-translation-search-with-aem-assets}

Smart Translation Search可跨AEM內容（資產和頁面）自動進行跨語言搜尋和探索，支援超過50種語言，並減少手動內容翻譯的需求。

>[!VIDEO](https://video.tv.adobe.com/v/21297?quality=12&learn=on)

AEM Smart Translation搜尋可讓使用者使用非英文辭彙在AEM中執行內容搜尋，以比對AEM中擁有同等英文辭彙的資產。

智慧型翻譯搜尋是套用至英文資產的AEM智慧型標籤的完美補充。

本影片假設 [AEM智慧型翻譯搜尋](smart-translation-search-technical-video-setup.md) 已設定。

## 智慧型翻譯搜尋如何運作 {#how-smart-translation-search-works}

![智慧型翻譯搜尋流程圖](assets/smart-translation-search-flow.png)

1. AEM使用者執行全文搜尋，提供當地語系化的搜尋字詞(例如 西班牙文中的&#39;man&#39;、&#39;hombre&#39;一詞)。
2. Apache Oak Machine Translation OSGi套件組合提供的Smart Translation Search會參與，並評估所提供的搜尋辭彙是否可使用註冊的語言套件進行翻譯。
3. 系統會收集步驟#2中的所有翻譯辭彙，並在內部擴充查詢，以納入這些辭彙作為搜尋辭彙。 如果針對AEM搜尋索引正常評估並找出相關相符專案，則此搜尋辭彙集擴大。
4. 系統會收集符合原始字詞(「hombre」)或翻譯字詞(「man」)的搜尋結果，並將使用者傳回作為搜尋結果。

## 其他资源{#additional-resources}

* [使用AEM Assets設定智慧型翻譯搜尋](smart-translation-search-technical-video-setup.md)
* [Apache Joshua語言套件](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
