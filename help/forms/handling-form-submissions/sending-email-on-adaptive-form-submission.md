---
title: 在最適化表單提交時傳送電子郵件
seo-title: Sending Email on Adaptive Form Submission
description: 使用傳送電子郵件元件，針對自適應表單提交傳送確認電子郵件
seo-description: Send confirmation email on adaptive form submission using the send email component
uuid: 6c9549ba-cb56-4d69-902c-45272a8fd17e
feature: Adaptive Forms
topics: authoring, integrations
audience: developer
doc-type: article
activity: use
discoiquuid: 1187357f-2f36-4a04-b708-44bb9c174fb5
topic: Development
role: Developer
level: Beginner
exl-id: 19c5aeec-2893-4ada-b6df-b80c4be2468a
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '214'
ht-degree: 3%

---

# 在最適化表單提交時傳送電子郵件 {#sending-email-on-adaptive-form-submission}

常見的動作之一，是在成功提交最適化表單時傳送確認電子郵件給提交者。 若要完成此操作，我們將選取「傳送電子郵件」作為提交動作。

您可以使用電子郵件範本，或只要輸入電子郵件的內文，如下方熒幕擷圖所示。

請注意在電子郵件中插入表單欄位值的語法。我們也可以選取設定屬性中的「包含附件」核取方塊，以在電子郵件中包含表單附件。

提交最適化表單時，收件者會收到電子郵件。

![SendEmail](assets/sendemailaction.gif)

## 所需設定 {#configurations-needed}

您必須設定Day CQ郵件服務。 您可透過將瀏覽器指向 [Felix設定管理員](http://localhost:4502/system/console/configMgr)

此熒幕擷圖顯示Adobe郵件伺服器的設定屬性。

![mailservice](assets/mailservice.png)

若要在您的伺服器上嘗試此方法，請遵循下列指示：

* [匯入資產](assets/timeoffrequest.zip) 使用封裝管理程式在AEM中與此文章相關聯。

* 開啟 [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).

* 填寫詳細資料。請務必在電子郵件欄位中提供有效的電子郵件地址。

* 提交表单.
