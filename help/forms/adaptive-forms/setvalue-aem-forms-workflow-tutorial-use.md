---
title: 在AEM Forms工作流程中使用setvalue
description: 在AEM Forms OSGI中設定最適化Forms提交資料的元素值
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 3919efee-6998-48e8-85d7-91b6943d23f9
last-substantial-update: 2020-01-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 0%

---

# 在AEM Forms工作流程中使用setvalue

在AEM Forms OSGI工作流程中，設定最適化Forms中提交資料的XML元素值。

![設定值](assets/setvalue.png)

用來設定值元件的LiveCycle，可讓您設定XML元素的值。

根據此值，使用XML填入表單時，您可以隱藏/停用表單的某些欄位或面板。

在AEM Forms OSGI中 — 我們必須撰寫自訂OSGi套件組合以在XML中設定值。 此套件組合已隨本教學課程提供。
我們使用AEM工作流程中的「程式步驟」。 我們會將「在XML中設定元素的值」OSGi套件組合與此程式步驟建立關聯。
我們需要傳遞兩個引數至設定值組合。 第一個引數是需要設定其值的XML元素的XPath。 第二個引數是需要設定的值。
例如，在上述熒幕擷圖中，我們將intialstep元素的值設為「N」。
根據此值，最適化Forms中的某些面板會隱藏或顯示。
在我們的範例中，我們有一個簡單的休假請求表單。 此表單的發起人填寫其姓名和休假日期。 提交時，此表單會前往「管理員」進行稽核。 當管理員開啟表單時，第一個面板上的欄位會停用。 這是因為我們已將XML中初始步驟元素的值設定為「N」。

根據初始步驟欄位值，我們顯示第二個面板，「管理員」可以在這裡核准或拒絕請求

請使用規則編輯器檢視針對「要求休假時間」欄位設定的規則。

若要在本機系統上部署資產，請遵循下列步驟：

* [部署Developing withserviceuser套件](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [部署範例套件組合](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 這是自訂OSGI套件組合，可讓您在提交的xml資料中設定元素的值

* [下載並解壓縮zip檔案的內容](assets/setvalueassets.zip)
* 將瀏覽器指向 [封裝管理員](http://localhost:4502/crx/packmgr/index.jsp)
* 匯入並安裝setValueWorkflow.zip。 此範例為工作流程模型。
* 將瀏覽器指向 [Forms和檔案](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 按一下建立 |檔案上傳
* 上傳TimeOfRequestForm.zip
* 開啟 [TimeOffRequestform](http://localhost:4502/content/dam/formsanddocuments/timeoffapplication/jcr:content?wcmmode=disabled)
* 填寫3個必填欄位並提交
* 以「管理員」身分登入AEM （如果尚未登入）
* 前往 [「AEM收件匣」](http://localhost:4502/aem/inbox)
* 開啟「檢閱休假要求」表單
* 請注意，第一個面板中的欄位已停用。 這是因為表單已由檢閱者開啟。 此外，請注意現在會顯示核准或拒絕請求的面板

>[!NOTE]
>
>您可以透過啟用記錄器來啟用偵錯記錄
>com.aemforms.setvalue.core.SetValueinXml
>將瀏覽器指向http://localhost:4502/system/console/slinglog

>[!NOTE]
>
>請確定最適化表單提交選項中的資料檔案路徑已設為「Data.xml」。 這是因為程式步驟會在裝載資料夾下尋找名為Data.xml的檔案
