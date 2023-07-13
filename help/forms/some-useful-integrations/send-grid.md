---
title: 将AEM Forms与SendGrid集成
description: 使用AEM Forms利用基于SengGrid云的电子邮件投放平台。
feature: Adaptive Forms
version: 6.4,6.5
kt: 13605
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2023-07-14T00:00:00Z
source-git-commit: cc24ebca488ea286e8a4605edfb39420c1c10022
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 0%

---

# 将AEM Forms与SendGrid集成

欢迎阅读本技术指南，我们将在此指南中探索使用AEM Forms中的SendGrid动态模板发送电子邮件的过程。 本指南旨在让您清楚地了解如何利用动态模板有效地个性化您的电子邮件内容。

动态模板允许您创建电子邮件模板，这些模板可以根据在自适应表单中捕获的数据向收件人显示不同的内容。 利用个性化变量，您可以提供有针对性的自定义电子邮件体验，从而引起受众的共鸣。

此外，我们将深入探讨Swagger文件的使用，通过该文件包含客户姓名和电子邮件地址，并选择适当的动态电子邮件模板，进一步个性化您的电子邮件。

按照本文档中的分步说明利用SendGrid动态模板和AEM Forms的强大功能，将电子邮件通信提升到新的参与度和相关性级别。 我们开始吧！

## 前提条件

在继续使用AEM Forms中的SendGrid动态模板发送电子邮件之前，请确保已满足以下先决条件：

1. **SendGrid帐户**：在以下位置注册SendGrid帐户： [https://sendgrid.com](https://sendgrid.com) 访问其电子邮件投放服务。 您需要帐户凭据才能将SendGrid与AEM Forms集成。
1. **熟悉如何创建数据源**：具备在AEM Forms中创建数据源的工作知识。 如有需要，请参阅有关以下内容的文档 [创建数据源](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) 以获取详细说明。
1. **熟悉表单数据模型**：了解AEM Forms中表单数据模型的概念。 如果需要，请查看文档，网址为 [创建表单数据模型](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html) 以确保您具备必要的了解。

在满足这些先决条件后，您将具备基本知识和资源，可以使用AEM Forms的SendGrid动态模板有效地发送电子邮件。

## 示例资源

本文提供的示例资源包括：

* **[Swagger文件](assets/SendGridWithDynamicTemplate.yaml)**：此文件允许您使用动态电子邮件模板发送电子邮件。 它提供了与SendGrid和AEM Forms集成以实现无缝电子邮件投放所需的规范和配置。

您可以随意利用提供的Swagger文件，作为使用动态模板实施电子邮件功能的参考或起点。

## 测试说明

要测试本指南中描述的功能，请执行以下步骤：

1. 下载 [swagger文件](assets/SendGridWithDynamicTemplate.yaml) 在assets文件夹中提供。
2. 使用下载的swagger文件和SendGrid凭据创建Restful数据源。
3. 创建基于Restful数据源的表单数据模型。
4. 调用 `mail/send` 根据您的要求POST表单数据模型的操作。 例如，您可以在单击按钮时触发电子邮件，或将其包含在AEM Forms工作流中。

服务的示例有效负载如下所示。 将占位符值替换为您自己的数据：

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

确保 `template_id` 与SendGrid动态电子邮件模板的ID相对应，且电子邮件地址有效并由SendGrid进行验证。 中的值 `personalizations` 部分允许您使用自适应表单中用户输入的数据对电子邮件进行个性化设置。

通过执行以下步骤并自定义提供的有效负载，您可以有效地测试SendGrid动态模板与AEM Forms的集成。

