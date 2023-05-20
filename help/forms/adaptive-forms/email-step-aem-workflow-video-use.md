---
title: 使用Forms Workflow的傳送電子郵件步驟
description: AEM Forms 6.4中引進了傳送電子郵件步驟。透過此步驟，我們可以建立業務流程或工作流程，讓您傳送包含或不包含附件的電子郵件。 以下影片會逐步介紹設定傳送電子郵件元件的步驟
feature: Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 21e58bbc-c1d6-4d41-a4d4-f522a3a5d4a7
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 1%

---

# 使用Forms Workflow的傳送電子郵件步驟 {#using-send-email-step-of-forms-workflow}

AEM Forms 6.4中引進了傳送電子郵件步驟。透過此步驟，我們可以建立業務流程或工作流程，讓您傳送包含或不包含附件的電子郵件。 以下影片會逐步說明設定傳送電子郵件元件的步驟。

>[!VIDEO](https://video.tv.adobe.com/v/21499?quality=12&learn=on)

在本文章中，我們將逐步引導您瞭解以下使用案例：

1. 使用者填寫休假請求表單
1. 表單提交時觸發AEM Workflow
1. AEM Workflow利用「傳送電子郵件」元件來傳送包含DoR作為附件的電子郵件

在使用「傳送電子郵件」步驟之前，請務必從以下位置設定Day CQ郵件服務： [configMgr](http://localhost:4502/system/console/configMgr). 提供您環境的特定值

![設定Day CQ郵件服務](assets/mailservice.png)

作為與本文相關資產的一部分，您將獲得以下內容

1. 將在提交時觸發工作流程的最適化表單
1. 將傳送包含DOR作為附件之電子郵件的範例工作流程
1. 建立中繼資料屬性的OSGi套件組合

若要讓範例在您的系統上執行，請執行下列動作：

1. [部署Developing withserviceuser套件](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [下載並安裝setvalue套件組合](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)此套件包含用於建立中繼資料屬性的程式碼，此程式碼是工作流程程式步驟的一部分。
1. [設定Day CQ郵件服務](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html)
1. [使用套件管理器將與此文章相關聯的資產匯入並安裝到CRX中](assets/emaildoraemformskt.zip)
1. 啟動 [最適化表單](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled). 填寫必填欄位並提交。
1. 您應會收到包含DocumentOfRecord作為附件的電子郵件

探索 [工作流程模型](http://localhost:4502/editor.html/conf/global/settings/workflow/models/emaildor.html)

檢視工作流程的流程步驟。 與程式步驟相關聯的自訂程式碼將建立中繼資料屬性名稱，並從提交的資料中設定其值。這些值隨後由傳送電子郵件元件使用。

>[!NOTE]
>
>在AEM Forms 6.5及更高版本中，您不需要此自訂程式碼來建立中繼資料屬性。 請使用AEM Workflow中的變數功能

請確定已按照以下熒幕擷取畫面設定傳送電子郵件元件的附件索引標籤
![傳送電子郵件附件索引標籤](assets/sendemailcomponentconfigure.jpg)「DOR.pdf」值必須和最適化表單提交選項中指定的記錄檔案路徑中指定的值相符。
