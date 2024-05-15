---
title: SMS双重身份验证
description: 添加额外的安全层以帮助在用户希望执行某些活动时确认用户的身份
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6317
topic: Development
role: Developer
level: Experienced
exl-id: c2c55406-6da6-42be-bcc0-f34426b3291a
last-substantial-update: 2021-07-07T00:00:00Z
duration: 115
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '557'
ht-degree: 0%

---

# 使用用户的手机号码验证用户

SMS双重身份验证（双重身份验证）是一种安全验证过程，它通过用户登录网站、软件或应用程序来触发。 在登录过程中，用户会自动向其手机号码发送包含唯一数字代码的短信。

有许多组织提供此服务，只要他们对REST API有详尽的文档记录，您就可以使用AEM Forms的数据集成功能轻松集成AEM Forms。 在本教程中，我使用 [Nexmo](https://developer.nexmo.com/verify/overview) 以演示SMS 2FA用例。

执行以下步骤，使用Nexmo Verify服务在AEM Forms中实施SMS 2FA。

## 创建开发人员帐户

创建开发人员帐户 [Nexmo](https://dashboard.nexmo.com/sign-in). 记下API密钥和API密钥。 调用Nexmo服务的REST API时需要这些密钥。

## 创建Swagger/OpenAPI文件

OpenAPI规范（以前称为Swagger规范）是适用于REST API的API描述格式。 OpenAPI文件允许您描述整个API，包括：

* 每个端点的可用端点(/users)和操作(GET/users，POST/users)
* 操作参数每个操作的输入和输出身份验证方法
* 联系信息、许可证、使用条款和其他信息。
* API规范可以使用YAML或JSON编写。 该格式简单易学，对人和机器均可读取。

要创建您的第一个swagger/OpenAPI文件，请按照 [OpenAPI文档](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms支持OpenAPI规范版本2.0 (fka Swagger)。

使用 [swagger编辑器](https://editor.swagger.io/) 创建swagger文件以描述用于发送和验证使用短信发送的OTP代码的操作。 swagger文件可以采用JSON或YAML格式创建。 完整的swagger文件可从以下位置下载： [此处](assets/two-factore-authentication-swagger.zip)

## 创建数据源

要将AEM/AEM Forms与第三方应用程序集成，我们需要 [创建数据源](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) 在云服务配置中。

## 创建表单数据模型

AEM Forms数据集成提供了直观的用户界面以供创建和使用 [表单数据模型](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). 表单数据模型依赖数据源交换数据。
完成的表单数据模型可以 [从此处下载](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

## 创建自适应表单

将表单数据模型的POST调用与您的自适应表单相集成，以验证用户在表单中输入的手机号码。 您可以自由创建自己的自适应表单，并根据要求使用表单数据模型的POST调用发送和验证OTP代码。

如果要将示例资源与API密钥一起使用，请执行以下步骤：

* [下载表单数据模型](assets/sms-2fa-fdm.zip) 并使用导入到AEM [包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* 下载自适应表单示例可以是 [从此处下载](assets/sms-2fa-verification-af.zip). 此示例表单使用了本文中提供的表单数据模型的服务调用。
* 将表单从导入AEM [Forms和文档UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 在编辑模式下打开表单。 打开以下字段的规则编辑器

![sms-send](assets/check-sms.PNG)

* 编辑与字段关联的规则。 提供适当的API密钥
* 保存表单
* [预览表单](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) 并测试功能
