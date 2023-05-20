---
title: AEM Forms與Marketo（第3部分）
description: 使用AEM Forms表單資料模型將AEM Forms與Marketo整合的教學課程。
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 7096340b-8ccf-4f5e-b264-9157232e96ba
source-git-commit: 38e0332ef2ef45a73a81f318975afc25600392a8
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 1%

---

# 設定資料來源

AEM Forms資料整合可讓您設定並連線至不同的資料來源。 下列是支援的現成可用型別。 不過，只需少量自訂，您也可以與其他資料來源整合。

1. 關聯式資料庫 — MySQL、Microsoft SQL Server、IBM DB2和OracleRDBMS
1. AEM使用者設定檔
1. RESTful Web服務
1. 以SOAP為基礎的網站服務
1. OData服務

為了整合AEM Forms與Marketo，我們使用RESTful網路服務。 整合的第一步是設定 [資料來源。](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) 請使用本教學課程中提供的swagger檔案。 下列熒幕擷圖顯示設定資料來源時需要指定的重要屬性。
![資料來源](assets/datasource.jfif)

「marketo.json」是Swagger檔案，在本教學課程的資產中提供給您。
屬性主機專屬於您的Marketo執行個體。
驗證型別為自訂，驗證實作必須與「Marketo的AemForms」相符。 （除非您在程式碼中變更此專案）。

## 创建表单数据模型

之後，設定資料來源的下一步是建立表單資料模型，此模型會以先前步驟中設定的資料來源為基礎。 若要建立表單資料模型，請遵循下列步驟：

將瀏覽器指向 [資料整合頁面。](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) 這會列出在您的AEM執行個體上建立的所有資料整合。

1. 按一下建立 |表單資料模型
1. 提供有意義的標題，例如FormsAndMarketo，然後按「下一步」
1. 選取在先前步驟中設定的資料來源，然後按一下「建立與編輯」以在編輯模式下開啟表單資料模型
1. 展開「FormsAndMarketo」節點。 展開服務節點
1. 選取第一個「取得」作業
1. 按一下新增選取的專案
1. 按一下「新增相關模型物件」對話方塊中的「全選」，然後按一下「新增」
1. 按一下「儲存」按鈕以儲存您的表單資料模型
1. Tab鍵移至「服務」標籤
1. 選取列出的唯一服務，然後按一下測試服務
1. 提供有效的leadId，然後按一下「測試」。 如果一切順利，您應該要回頭取得銷售機會詳細資訊，如下方熒幕擷圖所示
   ![testresults](assets/testresults.jfif)

## 后续步骤

[整合各項以進行測試](./part4.md)

