---
title: 在工作流程中使用表單資料模型服務作為步驟
description: 從AEM Forms 6.4開始，我們現在能使用表單資料模型做為AEM Workflow的一部分。 以下影片會逐步介紹在AEM Workflow中設定「表單資料模型」步驟所需的步驟。
feature: Workflow
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 0c77a853-fa71-46ac-8626-99bc69d6222d
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 0%

---

# 在工作流程中使用表單資料模型服務作為步驟 {#using-form-data-model-service-as-step-in-workflow}

從AEM Forms 6.4開始，我們現在能使用表單資料模型做為AEM Workflow的一部分。 以下影片會逐步介紹在AEM Workflow中設定表單資料模型步驟所需的步驟


>[!VIDEO](https://video.tv.adobe.com/v/21719?quality=12&learn=on)

若要在您的伺服器上測試此功能，請遵循下列指示
* [下載和部署setvalue套件組合](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 這是設定中繼資料屬性的自訂OSGI套件組合。
>!![NOTE]在AEM Forms 6.5及更高版本中，此功能可立即用於 [請在此處說明](form-data-model-service-as-step-in-aem65-workflow-video-use.md)

* 使用SampleRest.war檔案設定tomcat，如所述 [此處](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html).部署在Tomcat中的war檔案具有傳回申請人信用評分的代碼。 信用分數是介於200到800之間的隨機數字

* [使用封裝管理程式將資產匯入AEM](assets/invoke-fdm-as-service-step.zip)此套件包含下列專案：

   * 使用FDM步驟的工作流程模型。
   * FDM步驟中使用的表單資料模型。
   * 在提交時觸發工作流程的最適化表單。
* 開啟 [MortgageApplicationForm](http://localhost:4502/content/dam/formsanddocuments/loanapplication/jcr:content?wcmmode=disabled). 填寫詳細資料並提交。 在提交的表單上 [貸款應用程式工作流程](http://http://localhost:4502/editor.html/conf/global/settings/workflow/models/LoanApplication2.html) 觸發。

![ 工作流 ](assets/fdm-as-service-step-workflow.PNG).
如果信用評分超過500，工作流程會利用「或」分割元件，將應用模組傳送給管理員。 如果信用分數小於500，則會將應用模組傳送至cavery
