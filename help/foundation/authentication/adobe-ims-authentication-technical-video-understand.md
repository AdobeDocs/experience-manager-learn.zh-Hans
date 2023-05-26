---
title: 了解Adobe Managed Services上的Adobe IMS身份验证与AEM
description: Adobe Experience Manager引入了对AEM实例的Admin Console支持，以及在Managed Services上对AEM的基于Adobe IMS (Identity Management System)的身份验证。   此集成允许AEM Managed Services客户在单个统一的Web控制台中管理所有Experience Cloud用户。 可以将用户和组分配给与AEM实例关联的产品配置文件，从而授予对特定AEM实例的集中管理访问权限。
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
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 3%

---

# 了解Adobe Managed Services上的Adobe IMS身份验证与AEM{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager引入了对AEM实例的Admin Console支持，并为Managed Services上的AEMAdobe基于Identity Management System (IMS)的身份验证。   此集成允许AEM Managed Services客户在单个统一的Web控制台中管理所有Experience Cloud用户。 可将用户和组分配到与AEM实例相关联的产品配置文件，授予对特定AEM实例的集中管理访问权限。

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Adobe Experience Manager IMS身份验证支持仅适用于“内部”用户（作者、审阅者、管理员、开发人员等），不适用于网站访客等外部最终用户。
* [Admin Console](https://adminconsole.adobe.com/) 将AEM Managed Services客户表示为IMS组织，将AEM实例表示为产品上下文。 Admin Console系统和产品管理员可以定义和管理。
* AEM Managed Services将拓扑与Admin Console同步，在Product Context和AEM实例之间创建1对1映射。
* Admin Console中的产品配置文件确定用户可以访问哪些AEM实例。
* 身份验证支持包括客户符合SAML2的IDP，用于SSO。
* 仅支持Enterprise ID或Federated ID（适用于客户SSO）(不支持个人AdobeID)。

*&#42;Adobe Managed Services客户的AEM 6.4 SP3及更高版本支持此功能。*

## 最佳实践 {#best-practices}

### 在Admin Console中应用权限

应避免在Admin Console和Adobe Experience Manager中在用户级别应用权限和访问权限。

在中，应通过“产品上下文”级别的“Admin Console组”向用户授予访问权限。 用户组通常最好通过组织内的逻辑角色来表示，以提高组在Adobe Experience Cloud产品中的可重用性。

>[!NOTE]
>
> 如果使用AEMas a Cloud Service，请将Admin Console用户直接分配给产品配置文件。 AEMas a Cloud Service不支持Admin Console用户通过Admin Console用户组向产品配置文件转移权限。

### 在Adobe Experience Manager中应用权限

在Adobe Experience Manager中，与Adobe IMS同步的用户组应定期添加到 [AEM提供的用户组](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html)，这些组件预配置了相应的权限，以便在AEM中执行一组特定的任务。 不应将从Adobe IMS同步的用户直接添加到 [AEM提供的用户组](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html).
