---
title: 在工作流中捕获表单数据模型服务中的错误消息
description: 从AEM Forms 6.5.1开始，我们现在能够捕获在使用调用表单数据模型服务作为AEM工作流中的步骤时生成的错误消息。 工作流.
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 8cae155c-c393-4ac3-a412-bf14fc411aac
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# 在调用表单数据模型服务步骤中捕获错误消息

从AEM Forms 6.5.1开始，我们现在可以选择捕获错误消息并指定验证选项。 增强了“调用表单数据模型服务”步骤，以提供以下功能。

* 为3层验证（“关”、“基本”和“完全”）提供了一个选项，用于处理调用表单数据模型服务时遇到的异常。 这3个选项依次表示检查数据库特定要求的更严格版本。
   ![验证级别](assets/validation-level.PNG)

* 提供一个用于自定义工作流执行的复选框。 因此，用户现在可以灵活地继续执行工作流，即使调用表单数据模型步骤会引发异常也是如此。

* 存储由于验证异常而导致的错误的重要信息。 已合并了三个自动完成类型变量选择器来选择相关变量，以存储ErrorCode(String)、ErrorMessage(String)和ErrorDetails(JSON)。 但是，如果异常不是DermisValidationException，则ErrorDetails将设置为null。
   ![捕获错误消息](assets/fdm-error-details.PNG)

通过这些更改，调用表单数据模型服务步骤可确保输入值符合swagger文件中提供的数据约束。 例如，当accountId和余额值与swagger文件中指定的数据约束不兼容时，会引发以下错误消息。

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
