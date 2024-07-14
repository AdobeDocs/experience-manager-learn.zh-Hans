---
title: 使用SendGrid发送电子邮件
description: 触发包含已保存表单链接的电子邮件
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-13717
exl-id: 4b2d1e50-9fa1-4934-820b-7dae984cee00
duration: 37
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 1%

---

# 与SendGrid集成

AEM Forms数据集成允许配置不同的数据源并将其与AEM Forms连接。 它提供了一个直观的用户界面，用于跨连接的数据源创建业务实体和服务的统一数据表示架构。

我们使用SendGrid API发送了使用动态模板的电子邮件。 假定您熟悉SendGrid API，以便使用动态模板发送电子邮件。 作为本教程的一部分，为您提供了用于描述API的Swagger文件。

## 创建集成

请按照以下步骤创建AEM Forms与SendGrid之间的集成

* 使用[swagger文件](./assets/SendGridWithDynamicTemplate.yaml)创建RESTful数据源。 [请观看此视频，了解有关在AEM Forms中创建数据源的详细说明](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)
* 根据上一步中创建的数据源创建表单数据模型。[按照关于创建表单数据模型的详细文档](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate/use-form-data-model/create-form-data-models.html)进行操作。

为本教程创建的表单数据模型将作为文章资产的一部分包含。

### 后续步骤

[创建Azure存储集成](./create-fdm.md)
