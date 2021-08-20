---
title: AEM Forms和Adobe Campaign Standard快速入门
description: 使用AEM Forms表单数据模型将AEM Forms与Adobe Campaign Standard集成，以获取ACS促销活动配置文件信息等。
feature: 自适应Forms，表单数据模型
version: 6.3,6.4,6.5
topic: 开发
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 0%

---


# AEM Forms和Adobe Campaign Standard快速入门 {#getting-started-with-aem-forms-and-adobe-campaign-standard}

![formandcampaign](assets/helpx-cards-forms.png)

本教程将列出将AEM Forms与Adobe Campaign Standard(ACS)集成的各种用例。

ACS拥有丰富的API公开，使ACS能够与我们选择的技术进行交互。 在本教程中，我们将重点介绍AEM Forms与ACS的连接。

要将AEM Forms与ACS集成，您需要执行以下步骤：

* [在ACS实例上设置API访问。](https://docs.campaign.adobe.com/doc/standard/en/api/ACS_API.html#setting-up-api-access)
* 创建JSON Web令牌。
* 将JSON Web令牌与AdobeIdentity Management服务交换为访问令牌。
* 在授权HTTP标头中包含此访问令牌，并在对ACS实例的每个请求中包含X-API-Key。

要开始使用，请按照以下说明操作

* [下载并解压缩与本教程相关的资产。](assets/aem-forms-and-acs-bundles.zip)
* 使用[Felix Web控制台](http://localhost:4502/system/console/bundles)部署包
* 在Felix OSGi配置中为Adobe Campaign提供适当的设置。
* [按照本文所述创建服务用户](/help/forms/adaptive-forms/service-user-tutorial-develop.md)。确保部署与文章关联的OSGi包。
* 将ACS私钥存储在etc/key/campaign/private.key中。 您必须在etc/key下创建一个名为campaign的文件夹。
* [为服务用户“data”提供对campaign文件夹的读取访问权限。](http://localhost:4502/useradmin)
