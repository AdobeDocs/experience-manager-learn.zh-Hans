---
title: 了解Adobe Managed Services上使用AEM进行Adobe IMS身份验证
description: Adobe Experience Manager引入了对AEM实例的Admin Console支持，以及基于Adobe IMS(Identity Management系统)的AEM Managed Services身份验证。   此集成允许AEM Managed Services客户在一个统一的Web控制台中管理所有Experience Cloud用户。 可以将用户和组分配给与AEM实例关联的产品配置文件，从而授予对特定AEM实例的集中管理访问权限。
version: 6.4, 6.5
feature: User and Groups
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
topic: Architecture
role: Architect
level: Experienced
exl-id: 52dd8a3f-6461-4acb-87ca-5dd9567d15a6
last-substantial-update: 2022-10-01T00:00:00Z
thumbnail: KT-781.jpg
source-git-commit: a156877ff4439ad21fb79f231d273b8983924199
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 3%

---

# 了解Adobe Managed Services上使用AEM进行Adobe IMS身份验证{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager引入了对AEM实例和AdobeIdentity Management系统(IMS)的Admin Console支持，以便在Managed Services上对AEM进行身份验证。   此集成允许AEM Managed Services客户在一个统一的Web控制台中管理所有Experience Cloud用户。 可以将用户和组分配到与AEM实例关联的产品配置文件，从而授予对特定AEM实例的集中管理访问权限。

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Adobe Experience Manager IMS身份验证支持仅适用于“内部”用户（作者、审阅人、管理员、开发人员等），而不适用于外部最终用户（如网站访客）。
* [Admin Console](https://adminconsole.adobe.com/) 将AEM Managed Services客户表示为IMS组织，将AEM实例表示为产品上下文。 Admin Console系统和产品管理员可以定义和管理。
* AEM Managed Services将拓扑与Admin Console同步，从而创建Product Context与AEM实例之间的一对一映射。
* Admin Console中的产品配置文件确定用户可以访问哪些AEM实例。
* 身份验证支持包括客户符合SAML2的IDP，用于SSO。
* 仅支持Enterprise ID或Federated ID（用于客户SSO）(不支持个人AdobeID)。

*&#42;AEM 6.4 SP3及更高版本支持Adobe Managed Services客户使用此功能。*

## 最佳实践 {#best-practices}

### 在Admin Console中应用权限

在Admin Console和Adobe Experience Manager中，应避免在用户级别应用权限和访问。

在中，Admin Console用户应通过产品上下文级别的用户组来授予访问权限。 用户组通常最好通过组织内的逻辑角色来表示，以促进组在Adobe Experience Cloud产品中的可重用性。

>[!NOTE]
>
> 如果使用AEMas a Cloud Service，请将Admin Console用户直接分配给产品配置文件。 AEM as a Cloud Service不支持Admin Console用户之间通过Admin Console用户组传递权限到产品配置文件。

### 在Adobe Experience Manager中应用权限

在Adobe Experience Manager中，从Adobe IMS同步的用户组应当添加到 [AEM提供的用户组](https://experienceleague.adobe.com/docs/experience-manager-64/administering/security/security.html)，预配置了在AEM中执行特定任务集的相应权限。 不应将从Adobe IMS同步的用户直接添加到 [AEM提供的用户组](https://experienceleague.adobe.com/docs/experience-manager-64/administering/security/security.html).
