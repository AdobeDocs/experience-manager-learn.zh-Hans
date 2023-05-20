---
title: 篩選快速應用程式
description: 簡易的Express應用程式，可篩選使用內容片段模組化的WKND冒險。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11812
thumbnail: KT-11812.jpg
index: false
hide: true
hidefromtoc: true
recommendations: noCatalog, noDisplay
exl-id: b64f33ab-cd18-4cbc-a57e-baf505f1442a
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---

# 篩選快速應用程式

探索AEM Headless GraphQL API使用 [Express](https://expressjs.com/) 和 [Pug](https://pugjs.org/) 應用程式。 此Express應用程式會建立可依活動型別篩選的WKND冒險清單。

此程式碼使用Adobe的 [NodeJS的AEM Headless使用者端](https://github.com/adobe/aem-headless-client-nodejs#aem-headless-client-for-nodejs) 使用以Node.js為基礎的JavaScript來叫用持續性GraphQL查詢。 此應用程式使用 `wknd-shared/adventures-all` 持續查詢以收集所有冒險活動，並衍生可用活動型別清單。 當使用者選擇活動型別時，選擇的型別傳遞到 `wknd-shared/adventures-by-activity` 持久查詢並擷取冒險詳細資料，只針對指定活動型別的那些冒險。 冒險詳細資料可透過AEM擷取 `wknd-shared/adventures-by-slug` 持久查詢。

此程式碼：

+ 連線到AEM Publish服務，且不需要驗證
+ 使用WKND的持續查詢： `wknd-shared/adventures-all`， `wknd-shared/adventures-by-activity`、和 `wknd-shared/adventures-by-slug`
