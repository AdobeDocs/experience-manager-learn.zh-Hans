---
title: 在表單資料模型服務中擷取錯誤訊息，作為工作流程中的步驟
description: 從AEM Forms 6.5.1開始，我們現在能擷取使用叫用表單資料模型服務產生的錯誤訊息，做為AEM Workflow中的步驟。 工作流.
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 8cae155c-c393-4ac3-a412-bf14fc411aac
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# 在呼叫表單資料模型服務步驟中擷取錯誤訊息

從AEM Forms 6.5.1開始，我們現在可以選擇擷取錯誤訊息並指定驗證選項。 已增強叫用表單資料模型服務步驟，以提供下列功能。

* 提供3層驗證（「關閉」、「基本」和「完整」）的選項，以處理叫用表單資料模型服務時遇到的例外狀況。 這3個選項相繼表示檢查資料庫特定需求的更嚴格版本。
   ![validation-levels](assets/validation-level.PNG)

* 提供自訂工作流程執行的核取方塊。 因此，即使「叫用表單資料模型」步驟擲回例外狀況，使用者現在仍可靈活地繼續進行工作流程執行。

* 儲存因驗證例外狀況所引起錯誤的重要資訊。 已合併三個自動完成型別的變數選取器，以選取相關變數來儲存ErrorCode(String)、ErrorMessage(String)和ErrorDetails(JSON)。 不過，如果例外狀況不是DermisValidationException，ErrorDetails會設為null。
   ![擷取錯誤訊息](assets/fdm-error-details.PNG)

透過這些變更，叫用表單資料模型服務步驟可確保輸入值遵守swagger檔案中提供的資料限制。 例如，當accountId和餘額值不符合swagger檔案中指定的資料限制時，會擲回以下錯誤訊息。

```json
{
    "errorCode": "AEM-FDM-001-049"
    "errorMessage": "Input validations failed during operation execution"
    "violations": {
        "/accountId": ["numeric instance is greater than the required maximum (maximum: 20, found: 97)"],
        "/newAccount/balance": ["instance type (string) does not match any allowed primitive type (allowed: [\"integer\",\"number\"])"]
    }   
}
```
