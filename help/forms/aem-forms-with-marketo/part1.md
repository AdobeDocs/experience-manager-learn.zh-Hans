---
title: AEM Forms與Marketo（第1部分）
description: 使用AEM Forms表單資料模型將AEM Forms與Marketo整合的教學課程。
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 45047852-4fdb-4702-8a99-faaad7213b61
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 38e0332ef2ef45a73a81f318975afc25600392a8
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 0%

---

# AEM Forms與Marketo

Marketo是Adobe的一部分，提供著重於帳戶行銷的行銷自動化軟體，包括電子郵件、行動裝置、社交、數位廣告、網頁管理和分析。

現在使用AEM Forms的表單資料模型，我們可以將AEM表單與Marketo緊密整合。

[進一步瞭解表單資料模型](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

Marketo公開REST API，允許遠端執行系統的許多功能。 從建立程式到大量潛在客戶匯入，有許多選項可讓您以微調方式控制Marketo執行個體。 使用表單資料模型可輕鬆將AEM Forms與Marketo整合。

本教學課程將逐步引導您完成使用表單資料模型將AEM Forms與Marketo整合的步驟。 完成本教學課程後，您將會有OSGi套件組合，可針對Marketo執行自訂驗證。 您也將已使用提供的swagger檔案設定資料來源。

若要開始使用，強烈建議您熟悉先決條件一節中列出的下列主題。

## 先决条件

1. [AEM伺服器已安裝AEM Forms附加套件](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. 本機AEM開發環境
1. 熟悉表單資料模型
1. Swagger檔案的基本知識
1. 建立最適化Forms

**使用者端密碼ID和使用者端密碼金鑰**

Marketo與AEM Forms整合的第一步是取得使用API執行REST呼叫所需的API認證。 您將需要下列專案

1. client_id
1. client_secret
1. identity_endment
1. 驗證URL

[請依照官方Marketo檔案取得上述屬性。](https://developers.marketo.com/rest-api/) 或者，您也可以聯絡Marketo執行個體的管理員。

**開始之前**

[下載並解壓縮與本文相關的資產。](assets/aemformsandmarketo.zip) zip檔案包含下列內容：

1. BlankTemplatePackage.zip — 這是最適化表單範本。 使用封裝管理員匯入此專案。
1. marketo.json — 這是用來設定資料來源的swagger檔案。
1. MarketoAndForms.MarketoAndForms.core-1.0-SNAPSHOT.jar — 這是執行自訂驗證的套裝。 如果您無法完成本教學課程或您的套件無法如預期運作，歡迎使用此功能。

## 后续步骤

[建立自訂驗證](./part2.md)
