---
title: 在AEM Forms中認證檔案
description: 使用Docassurance服務來認證AEM Forms中的PDF檔案
feature: Document Security
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1471929f-d269-4adc-88ad-2ad3682305e1
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 1%

---

# 在AEM Forms中認證檔案

認證檔案可為PDF檔案及表單收件者提供其真實性和完整性的額外保證。

若要認證檔案，您可以在案頭上使用Acrobat DC，或使用AEM Forms Document Services作為伺服器上自動化程式的一部分。

本文提供範例OSGI套件組合，供您使用AEM Forms Document Services驗證pdf檔案。範例中使用的程式碼為 [此處提供](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

若要使用AEM Forms認證檔案，必須遵循下列步驟

## 正在新增憑證至信任存放區 {#adding-certificate-to-trust-store}

請依照下列步驟將憑證新增至AEM中的Keystore

* [初始化全域信任存放區](http://localhost:4502/libs/granite/security/content/truststore.html)
* [搜尋fd-service](http://localhost:4502/security/users.html) 使用者
* **您必須捲動結果頁面才能載入所有使用者，以尋找fd-service使用者**
* 連按兩下fd-service使用者以開啟使用者設定視窗
* 按一下「從Keystore檔案新增私密金鑰」。指定您的憑證專屬的別名和密碼
   ![add-certificate](assets/adding-certificate-keystore.PNG)
* 儲存您的變更

## 建立OSGI服務

您可以撰寫專屬的OSGi套件組合，並使用AEM Forms使用者端SDK實作服務以認證PDF檔案。 下列連結對於撰寫您自己的OSGi套件組合很有幫助

* [建立您的第一個OSGi套件](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [使用檔案服務API](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

或者，您也可以使用本教學課程資產隨附的範例套件組合。

>[!NOTE]
>
>範例套件組合使用名為「ares」的別名來認證檔案。 因此，使用此套件組合時，請確定您的別名名為「ares」

## 在本機系統上測試範例

* 下載並安裝 [自訂檔案服務套裝](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* 下載並安裝 [使用服務使用者套件進行開發](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [請確定您已在Apache Sling服務使用者對應程式服務中新增下列專案](http://localhost:4502/system/console/configMgr)

   **DevelopingWithServiceUser.core：getformsresourceresolver=fd-service** 如下方熒幕擷圖所示
   ![使用者對應程式](assets/user-mapper-service.PNG)
* [匯入範例最適化表單](assets/certify-pdf-af.zip)
* [匯入並安裝自訂提交](assets/custom-submit-certify.zip)
* [開啟最適化表單](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* 上傳需要認證的PDF檔案
   **可選**  — 指定您要在認證檔案時使用的簽名欄位
* 按一下「提交」。
* 應傳回已認證的PDF。
