---
title: AEM工作流程中的變數[Part2]
description: 在AEM工作流程中使用XML、JSON、ArrayList、檔案型別的變數
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: e7d3e0be-5194-47c2-a668-ce78e727986e
source-git-commit: 4b47daf82e27f6bea4be30e3cdd132f497f4c609
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 0%

---

# AEM Workflow中JSON型別的變數

從AEM Forms 6.5開始，我們現在可以在AEM Workflow中建立JSON型別的變數。 通常，如果您根據JSON結構描述提交最適化Forms至AEM Workflow，或想要儲存表單資料模型叫用作業的結果，則會建立JSON型別的變數。 以下影片將逐步說明在AEM工作流程中建立及使用JSON型別變數所需的步驟

**如果使用AEM Forms 6.5.0**

建立JSON型別的變數來擷取工作流程模型中提交的資料時，請勿將JSON結構描述與變數建立關聯。 這是因為當您提交以JSON結構描述為基礎的最適化表單時，提交的資料與JSON結構描述不相容。 JSON結構描述投訴資料包含在afData.afBoundData.data元素中。

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**如果使用AEM Forms 6.5.1及更高版本**

您可以在工作流程模型中將結構描述與JSON型別的變數對應。 然後，您可以使用結構描述瀏覽器將結構描述元素與工作流程模型中的字串/數字變數對應

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

若要讓資產在您的系統上運作，請遵循下列步驟：

* [使用封裝管理程式下載資產並將其匯入AEM](assets/jsonandstringvariable.zip)
* [探索工作流程模型](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) 瞭解工作流程中使用的變數
* [設定電子郵件服務](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [開啟最適化表單](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* 填寫詳細資料並提交表單
