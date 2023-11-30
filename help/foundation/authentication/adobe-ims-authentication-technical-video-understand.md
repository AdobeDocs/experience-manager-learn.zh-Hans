---
title: 了解AdobeManaged Services上的Adobe IMS身份验证与AEM
description: Adobe Experience Manager引入了对AEM实例的Admin Console支持，以及在Managed Services上对AEM的基于Adobe IMS (Identity Management System)的身份验证。   此集成允许AEM Managed Services客户在单个统一的Web控制台中管理所有Experience Cloud用户。 可以将用户和组分配到与AEM实例相关联的产品配置文件，从而授予对特定AEM实例的集中管理访问权限。
version: 6.4, 6.5
feature: User and Groups
doc-type: Technical Video
jira: KT-781
topic: Architecture
role: Architect
level: Experienced
exl-id: 52dd8a3f-6461-4acb-87ca-5dd9567d15a6
last-substantial-update: 2022-10-01T00:00:00Z
thumbnail: KT-781.jpg
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 4%

---

# 了解AdobeManaged Services上的Adobe IMS身份验证与AEM{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager引入了AEM实例的Admin Console支持，并为Managed Services上的AEMAdobe基于Identity Management System (IMS)的身份验证。   此集成允许AEM Managed Services客户在单个统一的Web控制台中管理所有Experience Cloud用户。 可以将用户和组分配到与AEM实例相关联的产品配置文件，从而授予对特定AEM实例的集中管理访问权限。

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Adobe Experience Manager IMS身份验证支持仅针对“内部”用户（作者、查看者、管理员、开发人员等），不适用于网站访客等外部最终用户。
* [Admin Console](https://adminconsole.adobe.com/) 将AEM Managed Services客户表示为IMS组织，将AEM实例表示为产品上下文。 Admin Console系统和产品管理员可以定义和管理。
* AEM Managed Services将您的拓扑与Admin Console同步，在产品上下文和AEM实例之间创建1对1映射。
* Admin Console中的产品配置文件确定用户可以访问哪些AEM实例。
* 身份验证支持包括用于SSO的符合SAML2标准的客户IDP。
* 仅支持Enterprise ID或Federated ID（适用于客户SSO）(不支持个人AdobeID)。

*&#42;AEM 6.4 SP3及更高版本支持AdobeManaged Services客户使用此功能。*

## 最佳实践 {#best-practices}

### 在Admin Console中应用权限

应避免在Admin Console和Adobe Experience Manager中在用户级别应用权限和访问权限。

在中，应通过“产品上下文”级别的“Admin Console组”向用户授予访问权限。 用户组通常最好通过组织内的逻辑角色来表示，以提高组在Adobe Experience Cloud产品中的可重用性。

>[!NOTE]
>
> 如果使用AEMas a Cloud Service，请将Admin Console用户直接分配给产品配置文件。 AEMas a Cloud Service不支持Admin Console用户通过Admin Console用户组对产品配置文件拥有过渡权限。

### 在Adobe Experience Manager中应用权限

在Adobe Experience Manager中，应将从Adobe IMS同步的用户组添加到 [AEM提供的用户组](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html)，这些组件预配置了相应的权限，以便在AEM中执行一组特定的任务。 从Adobe IMS同步的用户不应直接添加到 [AEM提供的用户组](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html).
