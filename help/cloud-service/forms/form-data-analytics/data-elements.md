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
exl-id: b9dc505d-72c8-4b6a-974b-fc619ff7c256
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '129'
ht-degree: 0%

---

# 建立適當的資料元素

在Tags屬性中，我們新增了兩個新資料元素（ApplicatesStateOfResidence和validationError）。

![Adaptive-form](assets/data_elements.png)

## ApplicatorStateOfResidence

此 **ApplicatorStateOfResidence** 資料元素的設定方式為：選取 **核心** 「擴充功能」下拉式清單中的 **自訂程式碼** 的資料元素型別，如下方熒幕擷取畫面所示
![申請人 — 國家 — 居所](assets/applicantstateofresidence.png)

下列自訂程式碼是用來從 **_state_** 最適化表單欄位。

```javascript
// use the GuideBridge API to access adaptive form elements
//The state field's SOM expression is used to access the state field
var ApplicantsStateOfResidence = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].state[0]").value;
_satellite.logger.log(" Returning  Applicants State Of Residence is "+ApplicantsStateOfResidence);
return ApplicantsStateOfResidence;
```

## validationError

此 **ValidationError** 資料元素的設定方式為：選取 **核心** 「擴充功能」下拉式清單中的 **自訂程式碼** 的資料元素型別，如下方熒幕擷取畫面所示

![validation-error](assets/validation-error.png)

下列自訂程式碼是用來設定validationError資料元素值。

```javascript
var validationError = "";
// Using GuideBridge API to access adaptive forms fields using the fields SOM expression
var tel = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].telephone[0]");
var email = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].email[0]");
_satellite.logger.log("Got tel in Tags custom script "+tel.isValid)
_satellite.logger.log("Got email in Tags custom script "+email.isValid)
if(tel.isValid == false)
{  
  validationError = "error: telephone number";
  _satellite.logger.log("Validation error is "+ validationError);
}

if(email.isValid == false)
{  
  validationError = "error: invalid email";
  _satellite.logger.log("Validation error is "+ validationError);
}

return validationError;
```
