---
title: 将AEM Forms与SendGrid集成
description: 使用AEM Forms利用基于SengGrid云的电子邮件投放平台。
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-13605
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2023-07-14T00:00:00Z
exl-id: 62b73f4b-69d8-4ede-9d57-3d6472d25d5a
duration: 118
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 0%

---

# 将AEM Forms与SendGrid集成

欢迎阅读本技术指南，我们将在其中探索使用AEM Forms中的SendGrid动态模板发送电子邮件的过程。 本指南旨在让您清楚地了解如何利用动态模板有效地个性化您的电子邮件内容。

动态模板允许您创建电子邮件模板，这些模板可以根据在自适应表单中捕获的数据向收件人显示不同的内容。 利用个性化变量，您可以提供有针对性的自定义电子邮件体验，从而引起受众的共鸣。

此外，我们将深入探讨Swagger文件的使用，该文件允许您通过包含客户姓名和电子邮件地址并选择适当的动态电子邮件模板来进一步个性化电子邮件。

按照本文档中的分步说明利用SendGrid动态模板和AEM Forms的强大功能，将电子邮件通信提升到新的参与度和相关性级别。 让我们开始吧！

## 先决条件

继续使用AEM Forms中的SendGrid动态模板发送电子邮件之前，请确保已满足以下先决条件：

1. **SendGrid帐户**：在[https://sendgrid.com](https://sendgrid.com)注册SendGrid帐户以访问其电子邮件投放服务。 您需要帐户凭据才能将SendGrid与AEM Forms集成。
1. **熟悉创建数据源**：具有在AEM Forms中创建数据源的工作知识。 如果需要，请参阅有关[创建数据源](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=zh-Hans)的文档以了解详细说明。
1. **熟悉表单数据模型**：了解AEM Forms中表单数据模型的概念。 如果需要，请查看有关[创建表单数据模型](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=zh-Hans)的文档，以确保您具有必要的了解。

满足这些先决条件后，您将具备基本知识和资源，能够使用AEM Forms的SendGrid动态模板有效发送电子邮件。

## 示例资源

本文提供的示例资源包括：

* **[Swagger文件](assets/SendGridWithDynamicTemplate.yaml)**：此文件允许您使用动态电子邮件模板发送电子邮件。 它提供了与SendGrid和AEM Forms集成以实现无缝电子邮件投放所需的规范和配置。

您可以随意利用提供的Swagger文件，作为使用动态模板实施电子邮件功能的参考或起点。

## 测试说明

要测试本指南中描述的功能，请执行以下步骤：

1. 下载在资源文件夹中提供的[swagger文件](assets/SendGridWithDynamicTemplate.yaml)。
2. 使用下载的swagger文件和SendGrid凭据创建Restful数据源。
3. 基于Restful数据源创建表单数据模型。
4. 根据您的要求调用表单数据模型的`mail/send` POST操作。 例如，您可以在单击按钮时触发电子邮件，或将其包含在AEM Forms工作流中。

服务的样本有效负载如下所示。 将占位符值替换为您自己的数据：

```json
{
    "sendgridpayload": {
        "from": {
            "email": "gs@xyz.com"
        },
        "personalizations": [{
            "to": [{
                "email": "johndoe@xyz.com"
            }],
            "dynamic_template_data": {
                "customerName": "John Doe"
            }
        }],
        "template_id": "d-72aau292a3bd60b5300c"
    }
}
```

请确保`template_id`对应于SendGrid动态电子邮件模板的ID，并且电子邮件地址有效并由SendGrid验证。 利用`personalizations`部分中的值，可使用自适应表单中的用户输入数据对电子邮件进行个性化设置。

通过执行以下步骤并自定义提供的有效负载，您可以有效地测试SendGrid动态模板与AEM Forms的集成。
