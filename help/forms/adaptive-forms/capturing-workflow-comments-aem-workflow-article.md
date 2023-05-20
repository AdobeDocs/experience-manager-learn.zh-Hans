---
title: 在最適化Forms Workflow中擷取工作流程註解
description: 在AEM Workflow中擷取工作流程備註
feature: Workflow
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: 5c250bbb-bac6-427d-8aca-1fbb1229e02c
last-substantial-update: 2020-10-10T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '375'
ht-degree: 0%

---

# 在最適化Forms Workflow中擷取工作流程註解{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[僅適用於AEM Forms 6.4。在AEM Forms 6.5中，請使用變數功能來達成此使用案例]

常見的要求是能夠包含任務稽核者輸入的註解，並透過電子郵件傳送給您。 在AEM Forms 6.4中，沒有現成的機制可擷取使用者輸入的註解，並在電子郵件中包含這些註解。

為了滿足此需求，提供了一個OSGi套件組合範例，可用於擷取註解並將這些註解儲存為工作流程中繼資料屬性。

下列熒幕擷圖顯示如何使用中的程式步驟 [AEM工作流程](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html) 擷取註解並將其儲存為中繼資料屬性。 「擷取工作流程註解」是需要在程式步驟中使用的Java類別名稱。 您必須傳遞將保留評論的中繼資料屬性名稱。 在下方熒幕擷圖中，managerComments是將儲存註解的中繼資料屬性。

![workflowcomments1](assets/workflowcomments1.gif)

若要在您的系統上測試此功能，請遵循下列步驟：
* [請確定工作流程中的程式步驟已設定為使用「擷取工作流程註解」](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [部署Developing withserviceuser套件](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [部署SetValue套裝](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 此套件包含範常式式碼，可擷取註解並將其儲存為中繼資料屬性

* [將與本文相關的資產下載並解壓縮至您的檔案系統](assets/capturecomments.zip) 資產包含工作流程模型和最適化表單範例。

* 使用封裝管理員將2個zip檔案匯入AEM

* [瀏覽至此URL以預覽表單](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* 填寫表單欄位並提交表單

* [檢查您的AEM收件匣](http://localhost:4502/aem/inbox)

* 從收件匣開啟工作並提交表單。 出現提示時，請輸入一些註解。

註解會儲存在名為的中繼資料屬性中 `managerComments` 在AEM存放庫中。 若要檢查註解，請以管理員身分登入crx。 工作流程例項會儲存在下列路徑中：

`/var/workflow/instances/server0`

選取適當的工作流程例項，並檢查中繼資料節點中的屬性managerComments。
