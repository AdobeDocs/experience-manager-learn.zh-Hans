---
title: 傳送最適化表單附件
description: 使用傳送電子郵件元件傳送最適化表單附件
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: bd9e1fc1-2fc7-452c-9a4a-2e16f6821760
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 2%

---

# 简介



常見的使用案例是使用AEM工作流程中的「傳送電子郵件」元件來傳送最適化表單附件。
客戶通常會壓縮表單附件，或使用傳送電子郵件元件以個別檔案的形式傳送附件。

## 以zip檔案傳送表單附件

為了完成使用案例，已撰寫自訂工作流程處理步驟。 在此自訂流程步驟中，將含有表單附件的zip檔案建立並儲存在名為的檔案的裝載資料夾下 *zipped_attachments.zip*

![傳送表單附件](assets/send-form-attachments.JPG)

## 個別傳送表單附件

為了完成此使用案例，已撰寫自訂工作流程處理步驟。 在此自訂程式步驟中，我們會填入ArrayList of Documents和ArrayList of Strings型別的工作流程變數。

![send-list-of-documents](assets/send-list-of-documents.JPG)

## 后续步骤

[壓縮表單附件](./custom-process-step.md)
