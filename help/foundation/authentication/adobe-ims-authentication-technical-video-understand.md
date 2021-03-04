---
title: 了解Adobe IMS身份验证(使用Adobe Managed Services上的AEM)
description: Adobe Experience Manager引入了对AEM实例的Admin Console支持以及针对Managed Services上的AEM的基于Adobe IMS(Identity Management系统)的身份验证。   此集成允许AEM Managed Services客户在一个统一的Web控制台中管理所有Experience Cloud用户。 可以将用户和用户组分配给与AEM实例关联的产品用户档案，从而授予对特定AEM实例的集中管理访问权。
version: 6.4, 6.5
feature: 用户和组
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
topic: 架构
role: 架构师
level: 富有经验
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---


# 了解Adobe IMS在Adobe Managed Services上使用AEM进行身份验证{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager引入了对AEM实例的Admin Console支持以及针对Managed Services上的AEM的基于Adobe IMS(Identity Management系统)的身份验证。   此集成允许AEM Managed Services客户在一个统一的Web控制台中管理所有Experience Cloud用户。 可以将用户和用户组分配给与AEM实例关联的产品用户档案，从而授予对特定AEM实例的集中管理访问权。

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Adobe Experience Manager IMS身份验证支持仅针对“内部”用户（作者、审阅者、管理员、开发人员等），而不针对外部最终用户(如网站访客)。
* [Admin ](https://adminconsole.adobe.com/) Console将AEM Managed Services客户表示为IMS组织，AEM实例表示为产品上下文。Admin Console系统和产品管理员可以定义和管理。
* AEM Managed Services将您的拓扑与Admin Console同步，在Product Context和AEM实例之间创建一对一映射。
* Admin Console中的产品用户档案将决定用户可以访问的AEM实例。
* 身份验证支持包括客户符合SAML2的IDP，用于SSO。
* 将仅支持Enterprise ID或Federated ID（对于客户SSO）(不支持个人AdobeID)。

**对于Adobe Managed Services客户，AEM 6.4 SP3及更高版本支持此功能。*

## 最佳实践{#best-practices}

### 在Admin Console中应用权限

在Admin Console和Adobe Experience Manager中应避免在用户级别应用权限和访问。

在Admin Console中，应通过Product Context级别上的用户组向用户授予访问权限。 用户组通常最好通过组织内的逻辑角色来表示，以提升组在Adobe Experience Cloud产品中的可重用性。

>[!NOTE]
>
> 如果使用AEM作为Cloud Service，请将Admin Console用户直接分配给产品用户档案。 AEM作为Cloud Service，不支持Admin Console用户之间通过Admin Console用户组到产品用户档案的传递权限。

### 在Adobe Experience Manager中应用权限

在Adobe Experience Manager中，从Adobe IMS同步的用户组应以术语添加到[AEM提供的用户组](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html)，后者预先配置了在AEM中执行特定任务集的适当权限。 不应将从Adobe IMS同步的用户直接添加到[AEM提供的用户组](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html)。
