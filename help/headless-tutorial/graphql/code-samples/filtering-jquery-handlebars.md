---
title: 使用jQuery和Handlebars篩選
description: 使用jQuery和Handlebars的JavaScript實作，該實作會篩選要顯示的WKND Adventures。.
version: Cloud Service
mini-toc-levels: 1
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11135
thumbnail: KT-11135.jpg
index: false
hide: true
hidefromtoc: true
exl-id: 75ffd84a-62b1-480f-b05f-3664f54bb171
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 0%

---

# 使用jQuery和Handlebars篩選

探索AEM Headless GraphQL API使用下列JavaScript應用程式篩選資料的能力： [jQuery](https://jquery.com/) 和 [Handlebars](https://handlebarsjs.com/). 此應用程式會建立可依活動型別篩選的WKND冒險清單。

此程式碼使用Adobe的 [適用於JavaScript的AEM Headless使用者端](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) 以叫用持續的GraphQL查詢。 此應用程式使用 `wknd-shared/adventures-all` 持續查詢以收集所有冒險活動，並衍生可用活動型別清單。 當使用者選擇活動型別時，選擇的型別傳遞到 `wknd-shared/adventures-by-activity` 持久查詢並擷取冒險詳細資料，只針對指定活動型別的那些冒險。

此程式碼：

+ 連線到AEM Publish服務，且不需要驗證
+ 使用WKND的持續查詢： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-activity`
