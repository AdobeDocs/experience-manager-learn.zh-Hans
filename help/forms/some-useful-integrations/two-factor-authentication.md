---
title: SMS双因素身份验证
description: 添加额外的安全层，帮助在用户想要执行某些活动时确认其身份
feature: integrations
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6317
translation-type: tm+mt
source-git-commit: 9f8c858197e44de020ab195373f30e3d38dfd2cc
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 0%

---



# 验证用户是否使用其手机号码

SMS双因素身份验证（双因素身份验证）是一种安全验证过程，通过用户登录网站、软件或应用程序触发。 在登录过程中，用户将自动向其移动号码发送包含唯一数字代码的SMS。

有许多组织提供此服务，只要他们有记录良好的REST API，您就可以使用AEM Forms的数据集成功能轻松集成AEM Forms。 为了本教程的目的，我使 [用Nexmo](https://developer.nexmo.com/verify/overview) 演示SMS 2FA用例。

按照以下步骤使用Nexmo Verify服务与AEM Forms一起实施SMS 2FA。

## 创建开发人员帐户

使用Nexmo创建开发人 [员帐户](https://dashboard.nexmo.com/sign-in)。 记下API密钥和API密钥。 需要这些密钥才能调用Nexmo服务的REST API。

## 创建Swagger/OpenAPI文件

OpenAPI规范（以前称为Swagger规范）是REST API的API描述格式。 OpenAPI文件允许您描述整个API，包括：

* 每个端点上的可用端点（/用户）和操作(GET/用户、POST/用户)
* 操作参数每个操作的输入和输出身份验证方法
* 联系信息、许可证、使用条款和其他信息。
* API规范可以用YAML或JSON编写。 该格式易于人和机器学习和读取。

要创建您的第一个Swagger/OpenAPI文件，请按照OpenAPI [文档操作](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms支持OpenAPI规范版本2.0(fka Swagger)。

使用Swagger [编辑器](https://editor.swagger.io/) ，创建您的Swagger文件，以描述使用SMS发送OTP代码并验证其操作。 Swagger文件可以采用JSON或YAML格式创建。 完整的Swagger文件可从此处下 [载](assets/two-factore-authentication-swagger.zip)

## 创建数据源

要将AEM/AEM Forms与第三方应用程序集成，我们需 [要在云服务配置](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) 中创建数据源。

## 创建表单数据模型

AEM Forms数据集成提供直观的用户界面，用于创建和使用表 [单数据模型](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html)。 表单数据模型依赖于数据源来交换数据。
完整的表单数据模型可从 [此处下载](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

## 创建自适应表单

将表单POST模型的调用与自适应表单集成，以验证用户在表单中输入的手机号码。 您可以自由创建自己的自适应表单，并根据您的要求使用表单数据模型的POST调用来发送和验证OTP代码。

如果要将示例资产与API密钥一起使用，请按照以下步骤操作：

* [下载表单数据模型](assets/sms-2fa-fdm.zip) ，并使用包管理器 [导入AEM](http://localhost:4502/crx/packmgr/index.jsp)
* 可从此处下载示例自 [适应表单](assets/sms-2fa-verification-af.zip)。 此示例表单使用作为本文一部分提供的表单数据模型的服务调用。
* 将表单从Forms和 [文档UI导入AEM](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 在编辑模式下打开表单。 打开以下字段的规则编辑器
   ![sms发送](assets/check-sms.PNG)
* 编辑与字段关联的规则。 提供适当的API密钥
* 保存表单
* [预览表单](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) ，并测试功能


