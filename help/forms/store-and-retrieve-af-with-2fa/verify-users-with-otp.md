---
title: 使用OTP验证用户
description: 使用OTP验证与应用程序编号关联的手机号码。
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6594
thumbnail: 6594.jpg
topic: Development
role: Developer
level: Experienced
exl-id: d486d5de-efd9-4dd3-9d9c-1bef510c6073
duration: 84
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '403'
ht-degree: 0%

---

# 使用OTP验证用户

SMS双重身份验证（双重身份验证）是一种安全验证过程，它通过用户登录网站、软件或应用程序来触发。 在登录过程中，用户会自动向其手机号码发送包含唯一数字代码的短信。

有许多组织提供此服务，只要他们对REST API有详尽的文档记录，您就可以使用AEM Forms的数据集成功能轻松集成AEM Forms。 在本教程中，我使用[Nexmo](https://developer.nexmo.com/verify/overview)演示了SMS 2FA用例。

执行以下步骤，使用Nexmo Verify服务在AEM Forms中实施SMS 2FA。

## 创建开发人员帐户

使用[Nexmo](https://dashboard.nexmo.com/sign-in)创建开发人员帐户。 记下API密钥和API密钥。 调用Nexmo服务的REST API时需要这些密钥。

## 创建Swagger/OpenAPI文件

OpenAPI规范（以前称为Swagger规范）是适用于REST API的API描述格式。 OpenAPI文件允许您描述整个API，包括：

* 每个端点(GET /users、POST /users)上的可用端点(/users)和操作
* 操作参数每个操作的输入和输出
身份验证方法
* 联系信息、许可证、使用条款和其他信息。
* API规范可以使用YAML或JSON编写。 该格式简单易学，对人和机器均可读取。

要创建您的第一个swagger/OpenAPI文件，请按照[OpenAPI文档](https://swagger.io/docs/specification/2-0/basic-structure/)操作

>[!NOTE]
> AEM Forms支持OpenAPI规范版本2.0 (fka Swagger)。

使用[swagger编辑器](https://editor.swagger.io/)创建您的swagger文件，以描述使用短信发送并验证OTP代码的操作。 swagger文件可以采用JSON或YAML格式创建。 可以从[此处](assets/two-factore-authentication-swagger.zip)下载已完成的swagger文件

## 创建数据Source

要将AEM/AEM Forms与第三方应用程序集成，我们需要使用云服务配置中的swagger文件[&#128279;](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)来REST数据源。 完成的数据源将作为本课程资产的一部分提供给您。

## 创建表单数据模型

AEM Forms数据集成提供了一个直观的用户界面，用于创建和使用[表单数据模型](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)。 表单数据模型依赖数据源交换数据。
可从此处[&#128279;](assets/sms-2fa-fdm.zip)下载已完成的表单数据模型

![fdm](assets/2FA-fdm.PNG)

[创建主表单](./create-the-main-adaptive-form.md)
