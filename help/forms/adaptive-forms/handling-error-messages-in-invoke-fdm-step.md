---
title: 在工作流中捕获表单数据模型服务中的错误消息
seo-title: 在工作流中捕获表单数据模型服务中的错误消息
description: 从AEM Forms6.5.1开始，我们现在能够捕获在调用表单数据模型服务作为AEM工作流中的步骤时生成的错误消息。 工作流.
seo-description: 从AEM Forms6.5.1开始，我们现在能够捕获在调用表单数据模型服务作为AEM工作流中的步骤时生成的错误消息。 工作流.
feature: workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.5.1,6.5.2
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 0%

---


# 调用表单数据模型服务步骤中捕获错误消息

从AEM Forms6.5.1开始，我们现在可以选择捕获错误消息并指定验证选项。 调用表单数据模型服务步骤已得到增强，可提供以下功能。

* 为3层验证（“OFF”、“BASIC”和“FULL”）提供一个选项，用于处理调用表单数据模型服务时遇到的异常。 这3个选项依次表示检查数据库特定要求的更严格版本。
   ![验证级别](assets/validation-level.PNG)

* 提供用于自定义工作流执行的复选框。 因此，用户现在可以灵活执行工作流，即使调用表单数据模型步骤引发异常也是如此。

* 存储由验证异常引起的错误的重要信息。 已合并三个自动完成类型的变量选择器，以选择相关变量来存储ErrorCode(String)、ErrorMessage(String)和ErrorDetails(JSON)。 但是，如果异常不是DermisValidationException，则ErrorDetails将设置为null。
   ![捕获错误消息](assets/fdm-error-details.PNG)

通过这些更改，调用表单数据模型服务步骤将确保输入值符合Swagger文件中提供的数据约束。 例如，当accountId和balance值与swagger文件中指定的数据约束不兼容时，将引发以下错误消息。

```json
{

"errorCode": "AEM-FDM-001-049"

"errorMessage": "Input validations failed during operation execution"

"violations": {

"/accountId": ["numeric instance is greater than the required maximum (maximum: 20, found: 97)"],

"/newAccount/balance": ["instance type (string) does not match any allowed primitive type (allowed: [\"integer\",\"number\"])"]

}

}
```


