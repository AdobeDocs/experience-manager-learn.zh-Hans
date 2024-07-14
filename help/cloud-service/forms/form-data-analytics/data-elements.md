---
title: 使用Adobe Analytics报告已提交的表单数据字段
description: 将AEM Forms CS与Adobe Analytics集成以报告表单数据字段
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
jira: KT-12557
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Formsas a Cloud Service" before-title="false"
exl-id: b9dc505d-72c8-4b6a-974b-fc619ff7c256
duration: 42
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 2%

---

# 创建数据元素

在Tags属性中，我们添加了两个新数据元素（ApplicationsStateOfResidence和validationError）。

![自适应表单](assets/data_elements.png)

## ApplicantStateOfResidence

通过在扩展下拉列表中选择&#x200B;**Core**&#x200B;并为数据元素类型选择&#x200B;**自定义代码**，配置了&#x200B;**ApplicantStateOfResidence**数据元素，如下面的屏幕快照所示
![申请人 — 国家 — 居住地](assets/applicantstateofresidence.png)

以下自定义代码用于捕获&#x200B;**_状态_**&#x200B;自适应表单字段中的值。

```javascript
// use the GuideBridge API to access adaptive form elements
//The state field's SOM expression is used to access the state field
var ApplicantsStateOfResidence = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].state[0]").value;
_satellite.logger.log("Returning  Applicants State Of Residence is "+ApplicantsStateOfResidence);
return ApplicantsStateOfResidence;
```

## validationError

通过在扩展下拉列表中选择&#x200B;**Core**&#x200B;并为数据元素类型选择&#x200B;**自定义代码**，配置了&#x200B;**ValidationError**&#x200B;数据元素，如下面的屏幕快照所示

![验证错误](assets/validation-error.png)

编写了以下自定义代码以设置`validationError`数据元素值。

```javascript
var validationError = "";
// Using GuideBridge API to access adaptive forms fields using the fields SOM expression
var tel = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].telephone[0]");
var email = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].email[0]");

_satellite.logger.log("Got tel in Tags custom script "+tel.isValid)
_satellite.logger.log("Got email in Tags custom script "+email.isValid)

if (tel.isValid == false) {  
  validationError = "error: telephone number";
  _satellite.logger.log("Validation error is "+ validationError);
}

if (email.isValid == false) {  
  validationError = "error: invalid email";
  _satellite.logger.log("Validation error is "+ validationError);
}

return validationError;
```

## 后续步骤

[创建规则](./rules.md)