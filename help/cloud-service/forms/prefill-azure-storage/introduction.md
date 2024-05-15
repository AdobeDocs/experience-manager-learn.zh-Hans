---
title: 实施自适应表单的保存和恢复功能
description: 了解如何从Azure存储帐户存储和检索自适应表单数据。
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-13717
exl-id: b40b0ef4-efa9-400e-82d8-aa0c7feb7be4
duration: 28
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 3%

---

# 简介

在本教程中，我们将实施一个简单的用例，即允许表单填充器保存并继续表单填充过程。 用例的流程如下所示

* 用户开始填写自适应表单。
* 用户保存了表单，并希望在以后继续填写表单。
* 用户收到一封电子邮件通知，其中包含指向已保存表单的链接。

## 先决条件

* 体验AEM Forms CS，尤其是创建表单数据模型的。
* 具有使用Cloud Manager部署代码的体验。
* 访问AEM Forms CS的云就绪实例。

要在AEM Forms CS中实施上述用例，您需要满足以下条件

* [AEM Forms CS Cloud就绪实例](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/intellij-and-aem-sync.html?lang=en#set-up-aem-author-instance)
* [Azure门户帐户](https://portal.azure.com/)
* [SendGrid帐户](https://sendgrid.com/)

### 后续步骤

[创建页面组件](./page-component.md)
