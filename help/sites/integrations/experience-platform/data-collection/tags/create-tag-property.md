---
title: 建立標籤屬性
description: 瞭解如何使用最低設定來建立標籤屬性，以便與AEM整合。 系統向使用者介紹標籤UI，讓他們瞭解擴充功能、規則和發佈工作流程。
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 5980
thumbnail: 38553.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: d5de62ef-a2aa-4283-b500-e1f7cb5dec3b
source-git-commit: 1d2daf53cd28fcd35cb2ea5c50e29b447790917a
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 1%

---

# 建立標籤屬性 {#create-tag-property}

瞭解如何使用最低設定來建立標籤屬性，以便與Adobe Experience Manager整合。 系統向使用者介紹標籤UI，讓他們瞭解擴充功能、規則和發佈工作流程。

>[!VIDEO](https://video.tv.adobe.com/v/38553?quality=12&learn=on)

## 標籤屬性建立

若要建立Tag屬性，請完成以下步驟。

1. 在瀏覽器中導覽至 [Adobe Experience Cloud首頁](https://experience.adobe.com/) 頁面並使用Adobe ID登入。

1. 按一下 **資料彙集** 應用程式來自 _快速存取_ Adobe Experience Cloud區段建立連結。

1. 按一下 **標籤** 左側導覽中的功能表專案，然後按一下 **新增屬性** 從右上角。

1. 使用為您的標籤屬性命名 **名稱** 必填欄位。 在「網域」欄位中，輸入您的網域名稱；如果使用AEMas a Cloud Service環境，請輸入 `adobeaemcloud.com` 並按一下 **儲存**.

   ![標籤屬性](assets/tag-properties.png)

## 建立新規則

按一下「 」中的「 」名稱，開啟新建立的Tag屬性 **標籤屬性** 檢視。 亦位於 _我最近的活動_ 標題中，您應該會看到核心擴充功能已新增至其中。 核心標籤擴充功能是預設的擴充功能，可提供頁面載入、瀏覽器、表單和其他事件型別等基本事件型別，請參閱 [核心擴充功能概觀](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/core/overview.html) 以取得詳細資訊。

規則可讓您指定當訪客與您的AEM網站互動時應該發生的事。 為了簡單起見，我們將兩則訊息記錄到瀏覽器主控台，示範資料收集標籤整合如何將JavaScript程式碼插入AEM網站，而不更新AEM專案程式碼。

若要建立規則，請完成下列步驟。

1. 按一下 **規則** 從 _製作_ 左側導覽的區段，然後按一下 **建立新規則**

1. 使用為您的規則命名 **名稱** 必填欄位。

1. 按一下 **新增** 從 _事件_ 區段，然後在 _事件設定_ 表單，在 **事件型別** 下拉式清單選取 _程式庫已載入（頁面頂端）_ 選項並按一下 **保留變更**.

1. 按一下 **新增** 從 _動作_ 區段，然後在 _動作設定_ 表單，在 **動作型別** 下拉式清單選取 _自訂程式碼_ 選項並按一下 **開啟編輯器**.

1. 在 _編輯程式碼_ 強制回應視窗，輸入下列JavaScript程式碼片段，然後按一下 **儲存**，最後按一下 **保留變更**.

   ```javascript
   console.log('Tags Property loaded, all set for...');
   console.log('capabilities such as capturing data, conversion tracking and delivering unique and personalized experiences');
   ```

1. 按一下 **儲存** 以完成規則建立程式。

   ![新規則](assets/new-rule.png)

## 新增程式庫並發佈

標籤屬性 _規則_ 使用程式庫啟動，請將程式庫視為包含JavaScript程式碼的套件。 按照以下步驟啟動新建立的規則。

1. 按一下 **發佈流程** 從 _發佈_ 左側導覽區段，然後按一下 **新增程式庫**

1. 使用為您的程式庫命名 **名稱** 欄位並選取 _Development(development)_ 選項 **環境** 下拉式清單。

1. 若要選取自標籤屬性建立後所有變更的資源，請按一下 **+新增所有變更的資源**. 此動作會將新建立的規則和核心擴充功能資源新增至程式庫。 最後按一下 **儲存並建置到開發環境**.

1. 為建置程式庫後 **開發** 泳道，使用 _橢圓_ 選取 **提交以進行核准**

1. 然後在 **已提交** 泳道使用 _橢圓_ 選取 **核准以發佈**，同樣地 **建置並發佈至生產環境** 在 **已核准** 泳道。

![已發佈程式庫](assets/published-library.png)


上述步驟會完成簡單的標籤屬性建立，此建立有規則可在頁面載入時將訊息記錄至瀏覽器主控台。 此外，也會透過建立程式庫來發佈規則和核心擴充功能。

## 后续步骤

[使用IMS連線AEM與標籤屬性](connect-aem-tag-property-using-ims.md)


## 其他资源 {#additional-resources}

* [建立標籤屬性](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html)
