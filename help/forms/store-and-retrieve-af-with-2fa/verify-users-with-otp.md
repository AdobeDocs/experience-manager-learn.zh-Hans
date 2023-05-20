---
title: 使用OTP驗證使用者
description: 使用OTP驗證與應用程式號碼關聯的行動電話號碼。
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6594
thumbnail: 6594.jpg
topic: Development
role: Developer
level: Experienced
exl-id: d486d5de-efd9-4dd3-9d9c-1bef510c6073
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '424'
ht-degree: 0%

---

# 使用OTP驗證使用者

SMS雙因素驗證(Dual Factor Authentication)是一種安全性驗證程式，透過使用者登入網站、軟體或應用程式來觸發。 在登入程式中，使用者會自動傳送SMS至包含唯一數字代碼的行動電話號碼。

有許多組織提供此服務，只要他們妥善記錄REST API，您就可以使用AEM Forms的資料整合功能，輕鬆整合AEM Forms。 在本教學課程中，我使用 [Nexmo](https://developer.nexmo.com/verify/overview) 示範SMS 2FA使用案例。

依照下列步驟，使用Nexmo Verify服務透過AEM Forms實作SMS 2FA。

## 建立開發人員帳戶

建立開發人員帳戶，使用 [Nexmo](https://dashboard.nexmo.com/sign-in). 記下API金鑰和API秘密金鑰。 呼叫Nexmo服務的REST API時需要這些金鑰。

## 建立Swagger/OpenAPI檔案

OpenAPI Specification （前身為Swagger Specification）是REST API的API說明格式。 OpenAPI檔案可讓您說明整個API，包括：

* 每個端點的可用端點(/users)和操作(GET/users、POST/users)
* 作業引數每個作業的輸入和輸出驗證方法
* 聯絡資訊、授權、使用條款及其他資訊。
* API規格可以用YAML或JSON撰寫。 該格式簡單易學，且可供人類和機器讀取。

若要建立您的第一個swagger/OpenAPI檔案，請遵循 [OpenAPI檔案](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms支援OpenAPI規格2.0版(fka Swagger)。

使用 [swagger編輯器](https://editor.swagger.io/) 建立您的swagger檔案，以說明傳送及驗證使用簡訊傳送之OTP代碼的作業。 Swagger檔案可以採用JSON或YAML格式建立。 完整的swagger檔案可從以下網址下載： [此處](assets/two-factore-authentication-swagger.zip)

## 建立資料來源

若要將AEM/AEM Forms與協力廠商應用程式整合，我們需要 [使用swagger檔案的REST型資料來源](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) 在雲端服務設定中。 完成的資料來源會作為本課程資產的一部分提供給您。

## 创建表单数据模型

AEM Forms資料整合提供直覺式使用者介面，用於建立和使用 [表單資料模型](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). 表單資料模型仰賴資料來源交換資料。
完成的表單資料模型可以是 [已從此處下載](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

[建立主要表單](./create-the-main-adaptive-form.md)
