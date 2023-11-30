---
title: OCR数据提取
description: 从政府颁发的文档中提取数据以填充表单。
feature: Barcoded Forms
version: 6.4,6.5
jira: KT-6679
topic: Development
role: Developer
level: Intermediate
exl-id: 1532a865-4664-40d9-964a-e64463b49587
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '708'
ht-degree: 1%

---

# OCR数据提取

自动从各种政府颁发的文档中提取数据以填充您的自适应表单。

有许多组织提供此服务，只要他们对REST API进行了详尽的记录，您就可以使用数据集成功能轻松与AEM Forms集成。 在本教程中，我使用 [ID分析器](https://www.idanalyzer.com/) 演示已上传文档的OCR数据提取。

执行以下步骤，使用ID分析器服务通过AEM Forms实施OCR数据提取。

## 创建开发人员帐户

创建开发人员帐户 [ID分析器](https://portal.idanalyzer.com/signin.html). 记下API密钥。 调用ID分析器服务的REST API时需要此密钥。

## 创建Swagger/OpenAPI文件

OpenAPI规范（以前称为Swagger规范）是适用于REST API的API描述格式。 OpenAPI文件允许您描述整个API，包括：

* 每个端点的可用端点(/users)和操作(GET/users，POST/users)
* 操作参数每个操作的输入和输出身份验证方法
* 联系信息、许可证、使用条款和其他信息。
* API规范可以使用YAML或JSON编写。 该格式简单易学，对人和机器均可读取。

要创建您的第一个swagger/OpenAPI文件，请按照 [OpenAPI文档](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms支持OpenAPI规范版本2.0 (fka Swagger)。

使用 [swagger编辑器](https://editor.swagger.io/) 创建swagger文件以描述用于发送和验证使用短信发送的OTP代码的操作。 swagger文件可以采用JSON或YAML格式创建。 完整的swagger文件可从以下位置下载： [此处](assets/drivers-license-swagger.zip)

## 定义swagger文件时的注意事项

* 需要定义
* 方法定义需要使用$ref
* 希望定义消耗量和生成部分
* 请勿定义内联请求正文参数或响应参数。 尽量实现模块化。 例如，不支持以下定义

```json
 "name": "body",
            "in": "body",
            "required": false,
            "schema": {
              "type": "object",
              "properties": {
                "Rollnum": {
                  "type": "string",
                  "description": "Rollnum"
                }
              }
            }
```

对requestBody定义的引用支持以下内容

```json
 "name": "requestBody",
            "in": "body",
            "required": false,
            "schema": {
              "$ref": "#/definitions/requestBody"
            }
```

* [示例Swagger文件以供您参考](assets/sample-swagger.json)

## 创建数据源

要将AEM/AEM Forms与第三方应用程序集成，我们需要 [创建数据源](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) 在云服务配置中。 请使用 [swagger文件](assets/drivers-license-swagger.zip) 以创建您的数据源。

## 创建表单数据模型

AEM Forms数据集成提供了直观的用户界面以供创建和使用 [表单数据模型](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). 使表单数据模型基于上一步中创建的数据源。

![fdm](assets/test-dl-fdm.PNG)

## 创建客户端库

我们需要获取已上传文档的base64编码字符串。 然后，此base64编码字符串将作为REST调用的参数之一传递。
可以下载客户端库 [从这里。](assets/drivers-license-client-lib.zip)

## 创建自适应表单

将表单数据模型的POST调用与您的自适应表单集成，以从表单中的用户上传文档中提取数据。 您可以自由创建自己的自适应表单，并使用表单数据模型的POST调用发送上传文档的base64编码字符串。

## 在您的服务器上部署

如果要将示例资源与API密钥一起使用，请执行以下步骤：

* [下载数据源](assets/drivers-license-source.zip) 并使用导入到AEM [包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* [下载表单数据模型](assets/drivers-license-fdm.zip) 并使用导入到AEM [包管理器](http://localhost:4502/crx/packmgr/index.jsp)
* [下载客户端库](assets/drivers-license-client-lib.zip)
* 下载自适应表单示例可以是 [从此处下载](assets/adaptive-form-dl.zip). 此示例表单使用了本文中提供的表单数据模型的服务调用。
* 将表单从导入AEM [Forms和文档UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 在中打开表单 [编辑模式。](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* 在apikey字段中指定API密钥作为默认值，并保存更改
* 打开Base 64字符串字段的规则编辑器。 当此字段的值更改时，请注意服务调用。
* 保存表单
* [预览表单](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled)，上传您的驾驶执照的正面图片
