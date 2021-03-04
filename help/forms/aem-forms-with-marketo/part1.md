---
title: AEM Forms with Marketo（第1部分）
seo-title: AEM Forms with Marketo（第1部分）
description: 使用AEM Forms表单数据模型将AEM Forms与Marketo集成的教程。
seo-description: 使用AEM Forms表单数据模型将AEM Forms与Marketo集成的教程。
feature: '"自适应Forms，表单数据模型"'
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
topic: 开发
role: 开发人员
level: 富有经验
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 0%

---


# AEM Forms与Marketo

Marketo是Adobe的一部分，它提供的营销自动化软件侧重于基于帐户的营销，包括电子邮件、移动、社交、数字广告、Web管理和分析。

利用AEM Forms的表单数据模型，我们现在可以将AEM Form与Market无缝集成。

[进一步了解表单数据模型](https://helpx.adobe.com/experience-manager/6-5/forms/using/data-integration.html)

Marketo公开一个REST API，它允许远程执行系统的许多功能。 从创建项目到批量导入，有许多选项允许对Marketo实例进行细粒度控制。 使用表单数据模型，将AEM Forms与Marketo集成非常简单。

本教程将指导您逐步完成使用表单数据模型将AEM Forms与Marketo集成所涉及的步骤。 完成本教程后，您将拥有一个OSGi捆绑包，它将针对Marketo进行自定义身份验证。 您还将使用提供的Swagger文件配置数据源。

要开始，强烈建议您熟悉“入门项目”部分中列出的以下主题。

## 先决条件

1. [安装AEM Forms Add的AEM服务器](/help/forms/adaptive-forms/installing-aem-form-on-windows-tutorial-use.md)
1. 本地AEM开发环境
1. 熟悉表单数据模型
1. Swagger文件的基本知识
1. 创建自适应Forms

**客户端密钥ID和客户端密钥**

将Marketo与AEM Forms集成的第一步是获取使用API进行REST调用所需的API凭据。 您将需要以下

1. client_id
1. client_secret
1. identity_endpoint
1. 身份验证URL。

[请按照正式的Marketo文档获取上述资产。](https://developers.marketo.com/rest-api/) 或者，您也可以与Marketo实例的管理员联系。

**开始前**

[下载并解压缩与本文相关的资源。](assets/aemformsandmarketo.zip) zip文件包含以下内容：

1. BlankTemplatePackage.zip — 这是自适应表单模板。 使用包管理器导入。
1. marketo.json — 这是用于配置数据源的swagger文件。
1. MarketoAndForms.MarketoAndForms.core-1.0-SNAPSHOT.jar — 这是执行自定义身份验证的包。 如果您无法完成教程或您的捆绑包未按预期方式工作，请随时使用它。
