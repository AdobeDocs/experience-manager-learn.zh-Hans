---
title: 對標籤實作除錯
description: 介紹對標籤實作進行偵錯的一些常用工具和技術。 瞭解如何使用瀏覽器的開發人員主控台和Experience Platform Debugger擴充功能，識別及疑難排解Tags實作的重大問題。
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 6047
thumbnail: 38567.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: 647447ca-3c29-4efe-bb3a-d3f53a936a2a
source-git-commit: 1d2daf53cd28fcd35cb2ea5c50e29b447790917a
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 0%

---

# 對標籤實作除錯 {#debug-tags-implementation}

介紹用於偵錯Tags實作的常見工具和技術。 瞭解如何使用瀏覽器的開發人員主控台和Experience Platform Debugger擴充功能，識別及疑難排解Tags實作的重大問題。

>[!VIDEO](https://video.tv.adobe.com/v/38567?quality=12&learn=on)

## 透過Satellite物件進行使用者端除錯

使用者端除錯有助於驗證Tag屬性規則載入或執行順序。 每當將Tag屬性新增至網站時， `_satellite` JavaScript物件存在於瀏覽器中，以促進使用者端事件和資料追蹤。

若要啟用使用者端除錯，請呼叫 `setDebug(true)` 上的方法 `_satellite` 物件。

1. 開啟瀏覽器主控台，然後執行下列命令。

   ```javascript
       _satellite.setDebug(true);
   ```

1. 重新載入AEM網站頁面，並確認主控台記錄檔顯示 _規則已引發_ 訊息如下。

   ![製作和發佈頁面上的標籤屬性](assets/satellite-object-debugging.png)

## 透過Adobe Experience Platform Debugger進行偵錯

Adobe提供Adobe Experience Platform Debugger [Chrome擴充功能](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) 和 [Firefox附加元件](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) 偵錯、瞭解及深入分析整合。

1. 開啟Adobe Experience Platform Debugger擴充功能，然後在發佈執行個體上開啟網站頁面

1. 在 **Adobe Experience Platform Debugger >摘要> Adobe Experience Platform標籤** 區段，確認您的Tag屬性詳細資訊，例如，名稱、版本、建置日期、環境和擴充功能。

   ![Adobe Experience Platform Debugger和標籤屬性詳細資料](assets/tag-property-details.png)

## 其他资源 {#additional-resources}

+ [Adobe Experience Platform Debugger簡介](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)

+ [Satellite物件參考](https://experienceleague.adobe.com/docs/experience-platform/tags/client-side/satellite-object.html)
