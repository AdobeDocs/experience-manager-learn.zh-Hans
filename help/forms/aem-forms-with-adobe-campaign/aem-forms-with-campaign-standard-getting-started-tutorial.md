---
title: AEM Forms和Adobe Campaign Standard入门
seo-title: AEM Forms和Adobe Campaign Standard入门
description: 将AEM Forms与Adobe Campaign Standard集成，使用AEM Forms表单数据模型获取ACS活动用户档案信息等。
seo-description: 将AEM Forms与Adobe Campaign Standard集成，使用AEM Forms表单数据模型获取ACS活动用户档案信息等。
uuid: 56450c9b-3752-4a64-b1b3-8c78e81f5921
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 89245554-7b99-4e7e-9810-52191f9ea365
translation-type: tm+mt
source-git-commit: 3b44a9e2341ce23f737e6ef75c67fadd9870d2ac
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 0%

---


# AEM Forms和Adobe Campaign Standard入门 {#getting-started-with-aem-forms-and-adobe-campaign-standard}

![formsandcampaign](assets/helpx-cards-forms.png)

本教程将列表将AEM Forms与Adobe Campaign Standard(ACS)集成的各种用例。

ACS拥有丰富的API集，允许ACS与我们选择的技术进行交互。 在本教程中，我们将重点介绍AEM Forms与ACS的接口。

要将AEM Forms与ACS集成，您需要执行以下步骤：

* [在ACS实例上设置API访问。](https://docs.campaign.adobe.com/doc/standard/en/api/ACS_API.html#setting-up-api-access)
* 创建JSON Web令牌。
* 将JSON Web令牌与AdobeIdentity Management服务交换为访问令牌。
* 将此访问令牌包含在授权HTTP头中，并在每个ACS实例请求中包含X-API-Key。

要开始，请按照以下说明操作

* [下载并解压缩与本教程相关的资源。](assets/aem-forms-and-acs-bundles.zip)
* 使用Felix Web控制台 [部署捆绑套件](http://localhost:4502/system/console/bundles)
* 在Felix OSGI配置中提供适当的Adobe Campaign设置。
* [创建本文所述的服务用户](/help/forms/adaptive-forms/service-user-tutorial-develop.md)。 确保部署与文章关联的OSGi捆绑包。
* 将ACS私钥存储在etc/key/campaign/private.key中。 您必须在etc/key下创建一个名为活动的文件夹。
* [为服务用户“活动”提供对数据文件夹的读取权限。](http://localhost:4502/useradmin)
