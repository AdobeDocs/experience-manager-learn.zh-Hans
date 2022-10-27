---
title: AEM Forms与Marketo（上）
description: 有关使用AEM Forms表单数据模型将AEM Forms与Marketo集成的教程。
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 45047852-4fdb-4702-8a99-faaad7213b61
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 0%

---

# AEM Forms与Marketo

Marketo是“Adobe”的一部分，它提供了“营销自动化”软件，该软件主要用于基于帐户的营销，包括电子邮件、移动设备、社交、数字广告、Web管理和分析。

使用AEM Forms的表单数据模型，我们现在可以将AEM表单与Marketo无缝集成。

[进一步了解表单数据模型](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

Marketo公开了一个REST API，它允许远程执行系统的许多功能。 从创建程序到批量导入潜在客户，有许多选项允许对Marketo实例进行细粒度控制。 使用表单数据模型，将AEM Forms与Marketo集成非常简单。

本教程将指导您完成使用表单数据模型将AEM Forms与Marketo集成时涉及的步骤。 完成本教程后，您将拥有一个OSGi包，该包将针对Marketo进行自定义身份验证。 您还将使用提供的swagger文件配置数据源。

要开始使用，强烈建议您熟悉先决条件部分中列出的以下主题。

## 先决条件

1. [安装了AEM Forms Add的AEM服务器包](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. 本地AEM开发环境
1. 熟悉表单数据模型
1. Swagger文件的基本知识
1. 创建自适应Forms

**客户端密钥ID和客户端密钥**

将Marketo与AEM Forms集成的第一步是获取使用API进行REST调用所需的API凭据。 您将需要以下信息

1. client_id
1. client_secret
1. identity_endpoint
1. 身份验证URL

[请按照官方的Marketo文档获取上述资产。](https://developers.marketo.com/rest-api/) 或者，您也可以联系Marketo实例的管理员。

**开始之前**

[下载并解压缩与本文相关的资产。](assets/aemformsandmarketo.zip) zip文件包含以下内容：

1. BlankTemplatePackage.zip — 这是自适应表单模板。 使用包管理器导入此文件。
1. marketo.json — 这是用于配置数据源的swagger文件。
1. MarketoAndForms.MarketoAndForms.core-1.0-SNAPSHOT.jar — 这是执行自定义身份验证的包。 如果您无法完成教程或包无法按预期工作，请随时使用此工具。
