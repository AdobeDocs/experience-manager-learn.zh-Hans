---
title: 疑難排解簽署多份檔案解決方案
description: 測試並疑難排解解決方案
feature: Adaptive Forms
version: 6.4,6.5
kt: 6960
thumbnail: 6960.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 99cba29e-4ae3-4160-a4c7-a5b6579618c0
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 0%

---

# 測試與疑難排解


## 預覽再融資表單

客戶服務代理填寫並提交時，使用案例就會觸發 [再融資表單](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled).

「簽署多個Forms」工作流程會在提交此表單時觸發，而客戶會收到電子郵件通知，其中包含開始表單填寫和簽署流程的連結。

## 在套件中填寫表單

呈現客戶以填寫並簽署套件中的第一個表單。 成功簽署表單後，客戶可以導覽至套件中的下一個表單。 填寫並簽署所有表單後，客戶會收到「**全部完成**」表單。

## 疑难解答

### 未產生電子郵件通知

電子郵件通知會由「簽署多份表單」工作流程中的「傳送電子郵件」元件傳送。 如果此工作流程中的任何步驟失敗，則會傳送電子郵件通知。 請確定工作流程中的自訂程式步驟是在MySQL資料庫中建立列。 如果正在建立列，請檢查您的Day CQ Mail Service組態設定

### 電子郵件通知中的連結無法運作

電子郵件通知中的連結會動態產生。 如果您的AEM伺服器不是在localhost：4502上執行，請在「簽署多個Forms」工作流程的「若要簽署的Forms商店」步驟引數中提供正確的伺服器名稱和連線埠

### 無法簽署表單

如果從資料來源擷取資料，而表單未正確填入，就可能發生這種情況。 檢查伺服器的stdout記錄。 fetchformdata.jsp會將一些有用的訊息寫出到stdout。

### 無法導覽至套件中的下一個表單

成功簽署封裝中的表單時，會觸發更新簽名狀態工作流程。 工作流程的第一步會更新資料庫中表單的簽名狀態。 請檢查表單的狀態是否從0更新為1。

### 未看到AllDone表單

當沒有更多表單可登入套件時，會向使用者顯示AllDone表單。如果您沒有看到AllDone表單，請檢查GetNextFormToSign.js檔案的第33行中使用的URL，它屬於 **getnextform** 使用者端程式庫。
