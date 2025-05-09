---
title: 集成AEM Forms和Adobe Campaign Standard
description: 使用AEM Forms表单数据模型将AEM Forms与Adobe Campaign Standard集成以获取ACS促销活动配置文件信息等。
feature: Adaptive Forms, Form Data Model
version: Experience Manager 6.4, Experience Manager 6.5
topic: Integrations, Development
role: Developer
level: Experienced
exl-id: e028837b-13d8-4058-ac25-ed095f49524c
badgeIntegration: label="集成" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
last-substantial-update: 2020-03-20T00:00:00Z
duration: 44
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 1%

---

# 集成AEM Forms和Adobe Campaign Standard

![formsandcampaign](assets/helpx-cards-forms.png)

了解如何将AEM Forms与Adobe Campaign Standard (ACS)集成。

ACS公开了一组丰富的API，使ACS可以与我们选择的技术接口。 在本教程中，我们将重点介绍AEM Forms与ACS的接口。

要将AEM Forms与ACS集成，您需要执行以下步骤：

* [在ACS实例上设置API访问。](https://experienceleague.adobe.com/docs/campaign-standard/using/working-with-apis/get-started-apis.html?lang=zh-Hans)
* 创建JSON Web令牌。
* 使用Adobe Identity Management服务交换JSON Web令牌以获取访问令牌。
* 在授权HTTP标头中包含此访问令牌，并在每个对ACS实例的请求中包含X-API-Key。

要开始配置，请按照以下说明操作

* [下载并解压缩与本教程相关的资源。](assets/aem-forms-and-acs-bundles.zip)
* 使用[Felix Web控制台](http://localhost:4502/system/console/bundles)部署包
* 在Felix OSGI配置中为Adobe Campaign提供相应的设置。
* [创建本文中提到的服务用户](/help/forms/adaptive-forms/service-user-tutorial-develop.md)。 确保部署与文章关联的OSGi捆绑包。
* 将ACS私钥存储在etc/key/campaign/private.key中。 您必须在etc/key下创建名为campaign的文件夹。
* [向服务用户“data”提供对Campaign文件夹的读取权限。](http://localhost:4502/useradmin)

## 后续步骤

[生成JWT和访问令牌](partone.md)
