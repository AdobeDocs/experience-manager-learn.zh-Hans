---
title: 測試解決方案 — 測試兩種方法所需的步驟
description: 將附件新增至表單以測試解決方案，並觸發工作流程以傳送電子郵件。
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: ce9b9203-b44c-4a52-821c-ae76e93312d2
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 0%

---

# 測試兩種方法所需的步驟

* 設定 [Day CQ郵件服務](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=en#configuring-the-mail-service) 若要從AEM Forms伺服器傳送電子郵件
* 部署 [表單附件](assets/formattachments.formattachments.core-1.0-SNAPSHOT.jar) 組合使用 [felix web主控台](http://localhost:4502/system/console/bundles)

## 將zip檔案以電子郵件附件的形式傳送



* 部署 [SendFormAttachmentsViaEmail工作流程。](assets/zipped-form-attachments-model.zip) 此工作流程會使用傳送電子郵件元件來傳送zipped_attachments.zip檔案，此檔案會由自訂流程步驟儲存在裝載資料夾下。 根據您的需求設定寄件者和收件者的電子郵件地址。
* 匯入 [範例表單](assets/zip-form-attachments-form.zip) 從 [Forms和檔案UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/zippformattachments/jcr:content?wcmmode=disabled) 並新增一些附件並提交表單。
* 應觸發工作流程，並傳送包含zip檔案的電子郵件通知。

## 以個別檔案傳送表單附件

* 部署 [SendForm工作流程](assets/send-form-attachments-model.zip) 此工作流程使用傳送電子郵件元件，以個別檔案的形式傳送表單附件。 根據您的需求設定寄件者和收件者的電子郵件地址。
* 匯入 [範例表單](assets/send-list-attachments-form.zip) 從 [Forms和檔案UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [預覽表單](http://localhost:4502/content/dam/formsanddocuments/sendlistofattachments/jcr:content?wcmmode=disabled) 並新增一些附件並提交表單。
* 應觸發工作流程，並傳送包含表單附件的電子郵件通知。
