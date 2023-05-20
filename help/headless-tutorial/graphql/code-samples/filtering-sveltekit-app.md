---
title: 簡單SvelteKit應用程式
description: 顯示使用內容片段模組化之WKND冒險的簡單SvelteKit應用程式。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 11811
thumbnail: KT-11811.jpg
index: false
hide: true
recommendations: noCatalog, noDisplay
hidefromtoc: true
exl-id: 2e5bd50e-c0d7-4292-8097-e0a17f41a91a
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '120'
ht-degree: 0%

---

# 正在篩選SvelteKit應用程式

探索AEM Headless GraphQL API使用 [SvelteKit](https://kit.svelte.dev/) 應用程式。 此SvelteKit應用程式會建立WKND冒險清單，您可以選取這些來顯示冒險的詳細資訊。

此程式碼使用Adobe的 [適用於JavaScript的AEM Headless使用者端](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) 從SvelteKit叫用持續性GraphQL查詢。 此應用程式使用 `wknd-shared/adventures-all` 持續查詢以收集所有冒險活動，並衍生可用活動型別清單。 如需冒險的詳細資訊，請前往 `wknd-shared/adventures-by-slug` 持久查詢。

此程式碼：

+ 連線到AEM Publish服務，且不需要驗證
+ 使用WKND的持續查詢： `wknd-shared/adventures-all` 和 `wknd-shared/adventures-by-slug`
