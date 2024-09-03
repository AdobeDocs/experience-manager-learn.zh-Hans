---
title: 集成AEM FormsCloud Service和Marketo（第2部分）
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
exl-id: 75e589fa-f7fc-4d0b-98c8-ce4d603ef2f7
source-git-commit: ba744f95f8d1f0b982cd5430860f0cb0945a4cda
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 3%

---

# 创建数据Source

Marketo的REST API使用双腿OAuth 2.0进行身份验证。我们可以使用上一步中下载的swagger文件轻松创建数据源

## 创建配置容器

* 登录AEM。
* 单击“工具”菜单，然后单击&#x200B;**配置浏览器**，如下所示

* ![工具菜单](assets/datasource3.png)

* 单击&#x200B;**创建**&#x200B;并提供一个有意义的名称，如下所示。 确保选择云配置选项，如下所示

* ![配置容器](assets/datasource4.png)

## 创建云服务

* 导航到工具菜单，然后单击云服务 — >数据源

* ![云服务](assets/datasource5.png)

* 选择在上一步中创建的配置容器，然后单击&#x200B;**创建**&#x200B;以创建新的数据源。提供有意义的名称，从“服务类型”下拉列表中选择RESTful服务，然后单击&#x200B;**下一步**
* ![新数据源](assets/datasource6.png)

* 上传swagger文件并指定特定于您的Marketo实例的授予类型、客户端ID、客户端密钥和访问令牌URL，如下面的屏幕快照所示。

* 测试连接，如果连接成功，请确保单击蓝色的&#x200B;**创建**&#x200B;按钮以完成创建数据源的过程。

* ![数据源配置](assets/datasource1.png)


## 后续步骤

[创建表单数据模型](./part3.md)
