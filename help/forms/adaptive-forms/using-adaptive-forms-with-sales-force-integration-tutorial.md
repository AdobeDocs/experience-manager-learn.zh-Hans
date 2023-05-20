---
title: 在AEM Forms 6.3和6.4中使用Salesforce設定DataSource
description: 使用表單資料模型整合AEM Forms與Salesforce
feature: Adaptive Forms, Form Data Model
topics: integrations
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 7a4fd109-514a-41a8-a3fe-53c1de32cb6d
last-substantial-update: 2020-02-14T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '892'
ht-degree: 0%

---

# 在AEM Forms 6.3和6.4中使用Salesforce設定DataSource{#configuring-datasource-with-salesforce-in-aem-forms-and}

## 前提条件 {#prerequisites}

在本文中，我們將逐步解說使用Salesforce建立資料來源的過程

本教學課程的先決條件：

* 捲動至本頁底部，下載swagger檔案並將其儲存至硬碟。
* 已啟用SSL的AEM Forms

   * [在AEM 6.3上啟用SSL的官方檔案](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/ssl-by-default.html)
   * [在AEM 6.4上啟用SSL的官方檔案](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/ssl-by-default.html)

* 您需要有Salesforce帳戶
* 您需要建立連線應用程式。 列出建立應用程式的官方檔案表單Salesforce [此處](https://help.salesforce.com/articleView?id=connected_app_create.htm&amp;type=0).
* 為應用程式提供適當的OAuth範圍（為了測試目的，我已選取所有可用的OAuth範圍）
* 提供回撥URL。 在我的案例中，回撥URL是

   * 如果您使用 **AEM Forms 6.3**，則回呼URL為https://gbedekar-w7-1:6443/etc/cloudservices/fdm/createlead.html。 在此URL中，建立的是我的表單資料模型的名稱。

   * 如果您使用**AEM Forms 6.4**，則回撥URL為https://gbedekar-w7-:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html

在此範例中，gbedekar -w7-1:6443是我的伺服器名稱，以及執行AEM的連線埠。

建立連線應用程式後，請留意 **使用者金鑰和秘密金鑰**. 在AEM Forms中建立資料來源時，需要這些資訊。

現在您已建立連線應用程式，您需要為需要在Salesforce中執行的操作建立Swagger檔案。 範例swagger檔案包含在可下載的資產中。 此Swagger檔案可讓您在提交最適化表單時建立「Lead」物件。 請探索此Swagger檔案。

下一步是在AEM Forms中建立資料來源。 請根據您的AEM Forms版本遵循下列步驟

## AEM Forms 6.3 {#aem-forms}

* 使用https通訊協定登入AEM Forms
* 輸入https://以導覽至雲端服務&lt;servername>：&lt;serverport> /etc/cloudservices.html，例如https://gbedekar-w7-1:6443/etc/cloudservices.html
* 向下捲動至「表單資料模型」。
* 按一下「顯示設定」。
* 按一下「+」以新增設定
* 選取「Rest Full Service」。 為設定提供有意義的標題和名稱。 例如，

   * 名稱： CreateLeadInSalesForce
   * 標題： CreateLeadInSalesForce

* 按一下「建立」

**在下一個畫面**

* 選取「檔案」作為Swagger來源檔案的選項。 瀏覽至您先前下載的檔案
* 選取驗證型別作為OAuth2.0
* 提供ClientID和使用者端密碼值
* OAuth Url是 —  **https://login.salesforce.com/services/oauth2/authorize**
* 重新整理記號Url - **https://na5.salesforce.com/services/oauth2/token**
* **Access Toke Url - https://na5.salesforce.com/services/oauth2/token**
* 授權範圍： ** api chatter_api完整id openid refresh_token visualforce web**
* 驗證處理常式：授權持有者
* 按一下「連線至OAUTH」。如果一切順利，您應該不會看到任何錯誤

使用Salesforce建立表單資料模型後，您就可以使用剛剛建立的資料來源建立表單資料整合。 建立表單資料整合的官方檔案為 [此處](https://helpx.adobe.com/aem-forms/6-3/data-integration.html).

請務必設定表單資料模型以包含POST服務，以便在SFDC中建立Lead物件。

您也必須為Lead物件設定Read and Write Service。 請參閱本頁底部的熒幕擷取畫面。

建立表單資料模型後，您可以根據此模型建立最適化Forms，並使用表單資料模型提交方法在SFDC中建立Lead。

## AEM Forms 6.4 {#aem-forms-1}

* 建立資料來源

   * [導覽至資料來源](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/global)

   * 按一下「建立」按鈕
   * 提供一些有意義的值

      * 名稱： CreateLeadInSalesForce
      * 標題： CreateLeadInSalesForce
      * 服務型別： RESTful服務
   * 按「下一步」
   * Swagger來源：檔案
   * 瀏覽並選取您在上一步中下載的swagger檔案
   * 驗證型別： OAuth 2.0。指定下列值
   * 提供ClientID和使用者端密碼值
   * OAuth Url是 —  **https://login.salesforce.com/services/oauth2/authorize**
   * 重新整理記號Url - **https://na5.salesforce.com/services/oauth2/token**
   * 存取Token Ur **l - https://na5.salesforce.com/services/oauth2/token**
   * 授權範圍： ** api chatter_api完整id openid refresh_token visualforce web**
   * 驗證處理常式：授權持有者
   * 按一下「連線至OAuth」按鈕。 若您發現任何錯誤，請檢閱上述步驟，以確保所有資訊皆已正確輸入。


使用SalesForce建立資料來源後，您就可以使用剛剛建立的資料來源建立表單資料整合。 的檔案連結為 [此處](https://helpx.adobe.com/experience-manager/6-4/forms/using/create-form-data-models.html)

請務必設定表單資料模型以包含POST服務，以便在SFDC中建立Lead物件。

您也必須為Lead物件設定Read and Write Service。 請參閱本頁底部的熒幕擷取畫面。

建立表單資料模型後，您可以根據此模型建立最適化Forms，並使用表單資料模型提交方法在SFDC中建立Lead。

>[!NOTE]
>
>確保swagger檔案中的url與您的地區相對應。 例如，範例swagger檔案中的url為「na46.salesforce.com」，因為帳戶是在北美建立。 最簡單的方式是登入您的Salesforce帳戶並檢查url 。

![sfdc1](assets/sfdc1.gif)

![sfdc2](assets/sfdc2.png)

[SampleSwaggerFile](assets/swagger-sales-force-lead.json)
