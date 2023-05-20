---
title: 簡易付費休假請求工作流程
description: 在AEM Workflow中隱藏和顯示最適化表單面板
uuid: 28ceb72b-24d9-488e-92af-7e85775dc682
feature: Adaptive Forms
topics: workflow
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
discoiquuid: 1c4822e6-76ce-446b-98cb-408900d68b24
topic: Development
role: Developer
level: Experienced
exl-id: 9342bd2f-2ba9-42ee-9224-055649ac3c90
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 0%

---

# 簡易付費休假請求工作流程

在本文中，我們將討論用於要求付費休假的簡單工作流程。 業務需求如下：

* 使用者A透過填寫最適化表單來要求休假。
* 表單會路由傳送給AEM管理員使用者（在實際情況中，它會路由傳送給提交者的管理員）
* 管理員會開啟表單。 管理員應該無法編輯提交者填入的任何資訊。
* 核准者區段應該對核准者可見(在此情況下，核准者是AEM管理員使用者)。

為了達到上述要求，我們使用隱藏欄位，稱為 **initialstep** ，其預設值設為「是」。提交表單時，工作流程中的第一個步驟會將initialstep的值設為「否」。 表單具有商業規則，可根據初始步驟值隱藏和顯示適當的區段。

**設定表單以觸發AEM Workflow**

>[!VIDEO](https://video.tv.adobe.com/v/28406?quality=12&learn=on)

**工作流程逐步說明**

>[!VIDEO](https://video.tv.adobe.com/v/28407?quality=12&learn=on)

**提交者的休假請求表單檢視**

![initialstep](assets/initialstep.gif)

**表單的核准者檢視**

![approverview](assets/approversview.gif)

在核准者檢視中，核准者無法編輯提交的資料。 還有一個新區段僅供核准者使用。

若要在您的系統上測試此工作流程，請遵循下列步驟：
* [下載並部署DevelopingWidthServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [下載和部署SetValue自訂OSGI套件組合](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
* [將與本文相關的資產匯入AEM](assets/helpxworkflow.zip)
* 開啟 [休假要求表單](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* 填寫詳細資料並提交
* 開啟 [收件匣](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html). 您應該會看到指派的新任務。 開啟表單。 提交者的資料應為唯讀，且應會顯示新的核准者區段。
* 探索 [工作流程模型](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)
* 探索程式步驟。 此步驟會將initialstep的值設為「否」。
