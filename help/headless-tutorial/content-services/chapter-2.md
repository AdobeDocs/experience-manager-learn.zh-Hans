---
title: 第2章 — 定義事件內容片段模型 — 內容服務
seo-title: Getting Started with AEM Content Services - Chapter 2 - Defining Event Content Fragment Models
description: AEM Headless教學課程的第2章涵蓋啟用和定義內容片段模型，用於定義標準化的資料結構和用於建立事件的製作介面。
seo-description: Chapter 2 of the AEM Headless tutorial covers enabling and defining Content Fragment Models used to define a normalized data structure and authoring interface for creating Events.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 8b05fc02-c0c5-48ad-a53e-d73b805ee91f
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '962'
ht-degree: 9%

---

# 第2章 — 使用內容片段模型

AEM內容片段模型會定義內容結構描述，這些結構描述可用來將AEM作者建立原始內容的過程製作成範本。 此方法類似於支架或表單式編寫。 內容片段的主要概念是所編寫的內容與呈現方式無關，這表示其旨在用於多管道使用，其中消費應用程式(不論是AEM、單頁應用程式或行動應用程式)會控制向使用者顯示內容的方式。

內容片段的主要問題是確保：

1. 系統會從作者處收集正確的內容
2. 內容能以結構化、容易理解的格式向使用者應用程式公開。

本章涵蓋啟用和定義內容片段模型，這些模型用於定義標準化資料結構和撰寫介面，以便建模和建立「事件」。

## 啟用內容片段模型

內容片段模型 **必須** 啟用方式： **[AEM [!UICONTROL 設定瀏覽器]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html)**.

如果內容片段模型為 **not** 已為設定啟用， **[!UICONTROL 建立] > [!UICONTROL 內容片段]** 按鈕不會針對相關AEM設定顯示。

>[!NOTE]
>
>AEM設定代表一組 [內容感知租使用者設定](https://sling.apache.org/documentation/bundles/context-aware-configuration/context-aware-configuration.html) 儲存於 `/conf`. AEM設定通常會與AEM Sites中管理的特定網站或負責內容子集（資產、頁面等）的業務單位建立關聯。 在AEM中。
>
>為了讓設定影響內容階層，必須透過 `cq:conf` 內容階層上的屬性。 (這是透過下列專案達成： [!DNL WKND Mobile] 中的設定 **步驟5** 下)。
>
>當 `global` 會使用設定，而設定會套用至所有內容，並且 `cq:conf` 不需要設定。
>
>請參閱 [[!UICONTROL 設定瀏覽器] 檔案](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html) 以取得詳細資訊。

1. 以具有適當許可權的使用者身分登入AEM作者，以修改相關設定。
   * 在本教學課程中， **管理員** 使用者可以使用。
1. 導覽至 **[!UICONTROL 工具] > [!UICONTROL 一般] > [!UICONTROL 設定瀏覽器]**
1. 點選 **資料夾圖示** 旁邊 **[!DNL WKND Mobile]** 以選取，然後點選 **[!UICONTROL 編輯] 按鈕** 左上角。
1. 選取 **[!UICONTROL 內容片段模型]**，然後點選 **[!UICONTROL 儲存並關閉]** 右上角。

   這會在具有下列專案的資產資料夾內容樹上啟用內容片段模型： [!DNL WKND Mobile] 設定已套用。

   >[!NOTE]
   >
   >此設定變更無法從 [!UICONTROL AEM設定] Web UI。 若要還原此設定：
   >    
   >    1. 開啟 [CRXDE Lite](http://localhost:4502/crx/de)
   >    1. 导航至 `/conf/wknd-mobile/settings/dam/cfm`
   >    1. 刪除 `models` 節點

   >    
   >任何在此設定下建立的現有內容片段模型都會刪除，且其定義會儲存在 `/conf/wknd-mobile/settings/dam/cfm/models`.

1. 套用 **[!DNL WKND Mobile]** 的設定 **[!DNL WKND Mobile]資產資料夾** 若要允許在該資產資料夾階層中建立內容片段模型的內容片段：

   1. 導覽至 **[!UICONTROL AEM] > [!UICONTROL 資產] > [!UICONTROL 檔案]**
   1. 選取 **[!UICONTROL WKND Mobile] 資料夾**
   1. 點選 **[!UICONTROL 屬性]** 按鈕以開啟 [!UICONTROL 資料夾屬性]
   1. 在 [!UICONTROL 資料夾屬性]，點選 **[!UICONTROL Cloud Services]** 標籤
   1. 驗證 **[!UICONTROL 雲端設定]** 欄位已設為 **/conf/wknd-mobile**
   1. 點選 **[!UICONTROL 儲存並關閉]** 以保留變更

>[!VIDEO](https://video.tv.adobe.com/v/28336?quality=12&learn=on)

>[!WARNING]
>
> __內容片段模型__ 已移動自 __「工具」>「資產」__ 至 __「工具」>「一般」__.

## 瞭解要建立的內容片段模型

在定義內容片段模式之前，請先檢閱我們將要推動的體驗，以確保擷取所有必要的資料點。 為此，我們將檢閱行動應用程式設計，並將設計元素對應至要收集的內容。

我們可以劃分定義事件的資料點，如下所示：

![建立內容片段模型](assets/chapter-2/design-to-model-mapping.png)

透過對應，我們可以定義用來收集並最終公開事件資料的內容片段。

## 建立內容片段模型

1. 導覽至 **[!UICONTROL 工具] > [!UICONTROL 一般] > [!UICONTROL 內容片段模型]**.
1. 點選 **[!DNL WKND Mobile]** 資料夾開啟。
1. 點選 **[!UICONTROL 建立]** 以開啟內容片段模式建立精靈。
1. 輸入 **[!DNL Event]** 作為 **[!UICONTROL 模型標題]** *（說明為選用）* 並點選 **[!UICONTROL 建立]** 以儲存。

>[!VIDEO](https://video.tv.adobe.com/v/28337?quality=12&learn=on)

## 定義內容片段模型的結構

1. 導覽至 **[!UICONTROL 工具] > [!UICONTROL 一般] > [!UICONTROL 內容片段模型] >[!DNL WKND]**.
1. 選取 **[!DNL Event]** 內容片段模型並點選 **[!UICONTROL 編輯]** 在頂端動作列中。
1. 從 **[!UICONTROL 資料型別] 標籤** 在右側，拖曳 **[!UICONTROL 單行文字輸入]** 放入左側放置區以定義 **[!DNL Question]** 欄位。
1. 確定新的 **[!UICONTROL 單行文字輸入]** 左側選取「 」，且 **[!UICONTROL 屬性] 標籤** ，即會在右側選取。 依照以下方式填入「屬性」欄位：

   * [!UICONTROL 呈现为] : `textfield`
   * [!UICONTROL 字段标签] : `Event Title`
   * [!UICONTROL 属性名称] : `eventTitle`
   * [!UICONTROL 最大長度] ：25
   * [!UICONTROL 必填] ： `Yes`

使用下列定義的輸入定義重複這些步驟，以建立其餘的事件內容片段模式。

>[!NOTE]
>
> 此 **屬性名稱** 欄位必須完全相符，因為Android應用程式的程式設計會關閉這些名稱。

### 事件描述

* [!UICONTROL 数据类型] : `Multi-line text`
* [!UICONTROL 字段标签] : `Event Description`
* [!UICONTROL 属性名称] : `eventDescription`
* [!UICONTROL 默认类型] : `Rich text`

### 事件日期和時間

* [!UICONTROL 数据类型] : `Date and time`
* [!UICONTROL 字段标签] : `Event Date and Time`
* [!UICONTROL 属性名称] : `eventDateAndTime`
* [!UICONTROL 必填] ： `Yes`

### 事件类型

* [!UICONTROL 数据类型] : `Enumeration`
* [!UICONTROL 字段标签] : `Event Type`
* [!UICONTROL 属性名称] : `eventType`
* [!UICONTROL 選項] ： `Art,Music,Performance,Photography`

### 票價

* [!UICONTROL 数据类型] : `Number`
* [!UICONTROL 呈现为] : `numberfield`
* [!UICONTROL 字段标签] : `Ticket Price`
* [!UICONTROL 属性名称] : `eventPrice`
* [!UICONTROL 类型] : `Integer`
* [!UICONTROL 必填] ： `Yes`

### 事件影像

* [!UICONTROL 数据类型] : `Content Reference`
* [!UICONTROL 呈现为] : `contentreference`
* [!UICONTROL 字段标签] : `Event Image`
* [!UICONTROL 属性名称] : `eventImage`
* [!UICONTROL 根路径] : `/content/dam/wknd-mobile/images`
* [!UICONTROL 必填] ： `Yes`

### 地點名稱

* [!UICONTROL 数据类型] : `Single-line text`
* [!UICONTROL 呈现为] : `textfield`
* [!UICONTROL 字段标签] : `Venue Name`
* [!UICONTROL 属性名称] : `venueName`
* [!UICONTROL 最大長度] ：20
* [!UICONTROL 必填] ： `Yes`

### 地點城市

* [!UICONTROL 数据类型] : `Enumeration`
* [!UICONTROL 字段标签] : `Venue City`
* [!UICONTROL 属性名称] : `venueCity`
* [!UICONTROL 選項] ： `Basel,London,Los Angeles,Paris,New York,Tokyo`

>[!VIDEO](https://video.tv.adobe.com/v/28335?quality=12&learn=on)

>[!NOTE]
>
>此 **[!UICONTROL 屬性名稱]** 表示 **兩者** 儲存此值的JCR屬性名稱以及JSON檔案中的索引鍵。 這應該是一個語意名稱，不會在內容片段模式的生命週期中變更。

完成內容片段模型的建立後，您應該會有如下的定義：


![事件內容片段模型](assets/chapter-2/event-content-fragment-model.png)

## 下一步

或者，您也可以安裝 [com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) AEM Author上的內容套件，透過 [AEM封裝管理員](http://localhost:4502/crx/packmgr/index.jsp). 此套件包含本教學課程中概述的設定和內容。

* [第3章 — 編寫事件內容片段](./chapter-3.md)
