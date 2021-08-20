---
title: 短信双重身份验证
description: 添加额外的安全层，以帮助在用户想要执行某些活动时确认其身份
feature: 自适应表单
version: 6.4,6.5
kt: 6317
topic: 开发
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '602'
ht-degree: 1%

---



# 使用用户的手机号码验证用户

短信双重身份验证（双重身份验证）是一种安全验证过程，通过用户登录网站、软件或应用程序来触发。 在登录过程中，系统会自动向用户的手机号码发送包含唯一数字代码的短信。

有许多组织提供此服务，只要他们有详实的REST API文档，您就可以使用AEM Forms的数据集成功能轻松集成AEM Forms。 在本教程中，我使用了[Nexmo](https://developer.nexmo.com/verify/overview)来演示SMS 2FA用例。

遵循以下步骤，使用Nexmo Verify服务在AEM Forms中实施SMS 2FA。

## 创建开发人员帐户

使用[Nexmo](https://dashboard.nexmo.com/sign-in)创建开发人员帐户。 记下API密钥和API密钥。 需要这些密钥才能调用Nexmo服务的REST API。

## 创建Swagger/OpenAPI文件

OpenAPI规范（以前称为Swagger规范）是REST API的API描述格式。 OpenAPI文件允许您描述整个API，包括：

* 每个端点上的可用端点(/users)和操作(GET/用户、POST/用户)
* 操作参数每个操作的输入和输出
身份验证方法
* 联系信息、许可证、使用条款和其他信息。
* API规范可以使用YAML或JSON编写。 该格式易于学习，对人类和机器都易读。

要创建您的第一个swagger/OpenAPI文件，请按照[OpenAPI文档](https://swagger.io/docs/specification/2-0/basic-structure/)操作

>[!NOTE]
> AEM Forms支持OpenAPI规范版本2.0(fka Swagger)。

使用[swagger editor](https://editor.swagger.io/)创建swagger文件，以描述使用短信发送和验证OTP代码的操作。 可以以JSON或YAML格式创建swagger文件。 可从[此处](assets/two-factore-authentication-swagger.zip)下载完成的swagger文件

## 创建数据源

要将AEM/AEM Forms与第三方应用程序集成，我们需要在云服务配置中[创建数据源](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)。

## 创建表单数据模型

AEM Forms数据集成提供了直观的用户界面，用于创建和使用[表单数据模型](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)。 表单数据模型依赖于数据源来交换数据。
可从此处](assets/sms-2fa-fdm.zip)下载完成的表单数据模型[

![fdm](assets/2FA-fdm.PNG)

## 创建自适应表单

将表单数据模型的POST调用与自适应表单相集成，以验证用户在表单中输入的手机号码。 您可以自由创建自己的自适应表单，并根据您的要求使用表单数据模型的POST调用来发送和验证OTP代码。

如果要将示例资产与API密钥一起使用，请执行以下步骤：

* [使用包管理器](assets/sms-2fa-fdm.zip) 将表单数据模型导入 [到AEM中](http://localhost:4502/crx/packmgr/index.jsp)
* 可从此处](assets/sms-2fa-verification-af.zip)下载示例自适应表单。 [此示例表单使用作为本文一部分提供的表单数据模型的服务调用。
* 将表单从[Forms和文档UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)导入AEM
* 在编辑模式下打开表单。 打开以下字段的规则编辑器

![短信发送](assets/check-sms.PNG)

* 编辑与字段关联的规则。 提供适当的API密钥
* 保存表单
* [预览](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) 表单并测试功能


