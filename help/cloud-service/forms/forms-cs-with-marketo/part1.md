---
title: 集成AEM Formsas a Cloud Service和Marketo
description: 了解如何使用AEM Forms表单数据模型集成AEM Forms和Marketo。
feature: Form Data Model,Integration
version: Cloud Service
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Formsas a Cloud Service" before-title="false"
last-substantial-update: 2024-07-24T00:00:00Z
jira: KT-15876
exl-id: c3145149-bfa4-4dcb-acde-c359e9348f99
source-git-commit: b4df652fcda0af5d01077b97aa7fa17cfe2abf4b
workflow-type: tm+mt
source-wordcount: '342'
ht-degree: 1%

---

# 集成AEM Forms和Marketo

Marketo是Adobe的一部分，它提供了营销自动化软件，侧重于基于帐户的营销，包括电子邮件、移动设备、社交、数字广告、Web管理和分析。

使用AEM Forms的表单数据模型，我们现在可以将AEM Form与Marketo无缝集成。

[了解有关表单数据模型的更多信息](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

Marketo会公开一个REST API，该API允许远程执行系统的多项功能。 从创建程序到批量引导导入，有许多选项允许对Marketo实例进行细粒度控制。 使用表单数据模型，可以非常轻松地将AEM Forms与Marketo集成。

本教程将指导您完成使用表单数据模型将AEM Forms与Marketo集成所涉及的步骤。 完成本教程后，您将获得一个OSGi捆绑包，该捆绑包将针对Marketo执行自定义身份验证。 您还将使用提供的swagger文件配置了数据源。

要开始配置，强烈建议您熟悉先决条件部分中列出的以下主题。

## 先决条件

1. 访问AEM Formsas a Cloud Service实例
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

* [下载并解压缩与本教程相关的资源](assets/marketo.zip)

zip文件包含以下内容：

1. marketo.json — 这是用于配置数据源的swagger文件。
1. 确保将marketo.json中的主机属性更改为指向您的marketo实例

## 后续步骤

[创建数据Source](./part2.md)
