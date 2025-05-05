---
title: 集成AEM Forms和Marketo
description: 了解如何使用AEM Forms表单数据模型集成AEM Forms和Marketo。
feature: Adaptive Forms, Form Data Model
version: Experience Manager 6.4, Experience Manager 6.5
topic: Integrations, Development
role: Developer
level: Experienced
exl-id: 45047852-4fdb-4702-8a99-faaad7213b61
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
last-substantial-update: 2020-03-20T00:00:00Z
duration: 77
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 1%

---

# 集成AEM Forms和Marketo


Marketo是Adobe的一部分，它提供了营销自动化软件，侧重于基于帐户的营销，包括电子邮件、移动设备、社交、数字广告、Web管理和分析。

使用AEM Forms的表单数据模型，我们现在可以将AEM Form与Marketo无缝集成。

[了解有关表单数据模型的更多信息](https://helpx.adobe.com/cn/experience-manager/6-5/forms/using/data-integration.html)

Marketo会公开一个REST API，该API允许远程执行系统的多项功能。 从创建程序到批量引导导入，有许多选项允许对Marketo实例进行细粒度控制。 使用表单数据模型，可以非常轻松地将AEM Forms与Marketo集成。

>[!NOTE]
>
>本教程专为AEM Forms 6.5量身定制。如果您希望将AEM Forms as a Cloud Service与Adobe Marketo Engage集成，请参阅[有关该集成的专用文档](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/forms/integrate/services/integrate-adaptive-form-with-market-engage/integrate-form-to-marketo-engage)。

本教程将指导您完成使用表单数据模型将AEM Forms与Marketo集成所涉及的步骤。 完成本教程后，您将获得一个OSGi捆绑包，该捆绑包将针对Marketo执行自定义身份验证。 您还将使用提供的swagger文件配置了数据源。

要开始配置，强烈建议您熟悉先决条件部分中列出的以下主题。

## 先决条件

1. [已安装带有AEM Forms附加组件包的AEM服务器](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. 本地AEM开发环境
1. 熟悉表单数据模型
1. Swagger文件的基本知识
1. 创建自适应Forms

**客户端密钥ID和客户端密钥**

Marketo与AEM Forms集成的第一步是获取使用API进行REST调用所需的API凭据。 您将需要以下项

1. client_id
1. clientsecret
1. identity_endpoint

[请按照Marketo官方文档获取上述资产。](https://developers.marketo.com/rest-api/)或者，您也可以联系Marketo实例的管理员。

**开始之前**

* [下载并解压缩与本教程相关的资源](assets/marketo-integration-assets.zip)

zip文件包含以下内容：

1. BlankTemplatePackage.zip — 这是自适应表单模板。 使用包管理器导入此项。
1. marketo.json — 这是用于配置数据源的swagger文件。
1. 确保将marketo.json中的主机属性更改为指向您的marketo实例

## 后续步骤

[创建数据Source](./part2.md)
