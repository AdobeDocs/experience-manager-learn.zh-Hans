---
title: SMS双因素身份验证
description: 添加额外的安全层，帮助在用户想要执行某些活动时确认其身份
feature: 自适应表单
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6317
topic: 开发
role: 开发人员
level: 富有经验
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '608'
ht-degree: 2%

---



# 验证用户是否使用其手机号码

SMS双因素身份验证（双因素身份验证）是一种安全验证过程，通过用户登录网站、软件或应用程序触发。 在登录过程中，用户会自动向其手机号码发送一个包含唯一数字代码的SMS。

有许多组织提供此服务，只要他们有详细的REST API文档，您就可以使用AEM Forms的数据集成功能轻松集成AEM Forms。 为了本教程的目的，我已使用[Nexmo](https://developer.nexmo.com/verify/overview)演示了SMS 2FA用例。

按照以下步骤使用Nexmo Verify服务与AEM Forms一起实施SMS 2FA。

## 创建开发人员帐户

使用[Nexmo](https://dashboard.nexmo.com/sign-in)创建开发人员帐户。 记下API密钥和API密钥。 需要这些密钥才能调用Nexmo服务的REST API。

## 创建Swagger/OpenAPI文件

OpenAPI规范（以前称为Swagger规范）是REST API的API描述格式。 OpenAPI文件允许您描述整个API，包括：

* 每个端点上的可用端点(/users)和操作(GET/users、POST/users)
* 操作参数每个操作的输入和输出
身份验证方法
* 联系信息、许可、使用条款和其他信息。
* API规范可以用YAML或JSON编写。 该格式易于人和机器学习和读取。

要创建您的第一个Swagger/OpenAPI文件，请按照[OpenAPI文档](https://swagger.io/docs/specification/2-0/basic-structure/)操作

>[!NOTE]
> AEM Forms支持OpenAPI规范2.0版(fka Swagger)。

使用[swagger editor](https://editor.swagger.io/)创建swagger文件，以描述使用SMS发送的OTP代码的发送和验证操作。 可以使用JSON或YAML格式创建Swagger文件。 可从[此处](assets/two-factore-authentication-swagger.zip)下载完成的Swagger文件

## 创建数据源

要将AEM/AEM Forms与第三方应用程序集成，我们需要在云服务配置中[创建数据源](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)。

## 创建表单数据模型

AEM Forms数据集成提供了直观的用户界面，用于创建和使用[表单数据模型](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html)。 表单数据模型依赖数据源来交换数据。
完整的表单数据模型可从此处](assets/sms-2fa-fdm.zip)下载[

![fdm](assets/2FA-fdm.PNG)

## 创建自适应表单

将表单POST模型的调用与自适应表单集成，以验证用户在表单中输入的移动电话号码。 您可以根据自己的要求创建自己的自适应表单并使用表单数据模型的POST调用来发送和验证OTP代码。

如果您希望将示例资产与您的API密钥一起使用，请执行以下步骤：

* [使用包管理器](assets/sms-2fa-fdm.zip) 将表单数据模型导入 [到AEM中](http://localhost:4502/crx/packmgr/index.jsp)
* 下载示例自适应表单可从此处](assets/sms-2fa-verification-af.zip)下载。 [此示例表单使用作为本文一部分提供的表单数据模型的服务调用。
* 将表单从[Forms和文档 UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)导入AEM
* 在编辑模式下打开表单。 打开以下字段的规则编辑器

![sms发送](assets/check-sms.PNG)

* 编辑与字段关联的规则。 提供适当的API密钥
* 保存表单
* [预览](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) 表单并测试功能


