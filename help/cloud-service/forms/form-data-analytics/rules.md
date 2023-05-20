---
title: 使用Adobe Analytics提交表單資料欄位的相關報告
description: 將AEM Forms CS與Adobe Analytics整合以報告表單資料欄位
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 12557
exl-id: 9982e041-fff7-4be6-91c9-e322d2fd3e01
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---

# 定義規則

在Tags屬性中，我們建立了2個新的 [規則](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/add-data-elements-rules.html) (**欄位驗證錯誤和FormSubmit**)。

![Adaptive-form](assets/rules.png)


## 欄位驗證錯誤

此 **欄位驗證錯誤** 每當最適化表單欄位中出現驗證錯誤時，就會觸發規則。 例如，在我們的表單中，如果電話號碼或電子郵件不是預期的格式，則會顯示驗證錯誤訊息。

欄位驗證錯誤規則是透過將事件設定為來設定 _**Adobe Experience Manager Forms-Error**_ 如熒幕擷取畫面所示



![申請人 — 國家 — 居所](assets/field_validation_error_rule.png)

Adobe Analytics — 設定變數的設定如下

![設定動作](assets/field_validation_action_rule.png)

## 表單提交規則

每次成功提交最適化表單時，就會觸發表單提交規則。

表單提交規則是使用 _**Adobe Experience Manager Forms — 提交**_ 事件

![form-submit-rule](assets/form-submit-rule.png)

在表單提交規則中，資料元素的值 _**ApplicatesStateOfResidence**_ 對應至prop5，而資料元素FormTitle的值對應至prop8。

Adobe Analytics — 設定變數的設定如下。
![form-submit-rule-set-variables](assets/form-submit-set-variable.png)

當您準備好要測試您的標籤程式碼時，[發佈您對標籤所做的變更](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/publishing-flow.html) 使用發佈流程。
