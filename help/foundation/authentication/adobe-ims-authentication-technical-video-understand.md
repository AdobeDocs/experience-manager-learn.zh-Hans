---
title: 了解Adobe Managed Services上的AdobeIMS身份验证和AEM
description: Adobe Experience Manager引入了对AEM实例的Admin Console支持，以及基于AEM on Managed Services身份验证的AdobeIMS(Identity Management系统)。   此集成允许AEM Managed Services客户在一个统一的Web控制台中管理所有Experience Cloud用户。 可以将用户和组分配给与AEM实例关联的产品配置文件，从而授予对特定AEM实例的集中管理访问权限。
version: 6.4, 6.5
feature: 用户和群组
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
topic: 架构
role: Architect
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 0%

---


# 了解Adobe Managed Services上的AdobeIMS身份验证和AEM{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager引入了对AEM实例的Admin Console支持，以及基于AEM on Managed Services身份验证的AdobeIMS(Identity Management系统)。   此集成允许AEM Managed Services客户在一个统一的Web控制台中管理所有Experience Cloud用户。 可以将用户和组分配到与AEM实例关联的产品配置文件，从而授予对特定AEM实例的集中管理访问权限。

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Adobe Experience Manager IMS身份验证支持仅适用于“内部”用户（作者、审阅人、管理员、开发人员等），而不适用于外部最终用户（如网站访客）。
* [Admin ](https://adminconsole.adobe.com/) Console将AEM Managed Services客户表示为IMS组织，将AEM实例表示为产品上下文。Admin Console系统和产品管理员可以定义和管理。
* AEM Managed Services将拓扑与Admin Console同步，从而创建Product Context与AEM实例之间的一对一映射。
* Admin Console中的产品配置文件将确定用户可以访问哪些AEM实例。
* 身份验证支持包括客户符合SAML2的IDP，用于SSO。
* 将仅支持Enterprise ID或Federated ID（用于客户SSO）(不支持个人AdobeID)。

** AEM 6.4 SP3及更高版本支持Adobe Managed Services客户使用此功能。*

## 最佳实践 {#best-practices}

### 在Admin Console中应用权限

在Admin Console和Adobe Experience Manager中，应避免在用户级别应用权限和访问。

在Admin Console中，应通过产品上下文级别的用户组向用户授予访问权限。 用户组通常最好通过组织内的逻辑角色来表示，以促进组在各个Adobe Experience Cloud产品中的可重复使用性。

>[!NOTE]
>
> 如果使用AEM作为Cloud Service，请将Admin Console用户直接分配给产品配置文件。 AEM as a Cloud Service不支持将Admin Console用户之间通过Admin Console用户组传递到产品配置文件的权限。

### 在Adobe Experience Manager中应用权限

在Adobe Experience Manager中，应将从AdobeIMS同步的用户组添加到[AEM提供的用户组](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html)中，这些用户组预配置了在AEM中执行特定任务集的相应权限。 不应将从AdobeIMS同步的用户直接添加到[AEM提供的用户组](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html)。
