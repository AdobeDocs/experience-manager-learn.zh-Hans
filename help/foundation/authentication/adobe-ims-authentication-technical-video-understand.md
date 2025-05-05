---
title: 了解Adobe Managed Services上使用AEM进行Adobe IMS身份验证
description: Adobe Experience Manager引入了Admin Console对AEM实例的支持，并针对Managed Services上的AEM引入了基于Adobe IMS (Identity Management System)的身份验证。   此集成允许AEM Managed Services客户在单个统一的Web控制台中管理所有Experience Cloud用户。 可以将用户和组分配到与AEM实例关联的产品配置文件，从而授予对特定AEM实例的集中管理访问权限。
version: Experience Manager 6.4, Experience Manager 6.5
feature: User and Groups
doc-type: Technical Video
jira: KT-781
topic: Architecture
role: Architect
level: Experienced
exl-id: 52dd8a3f-6461-4acb-87ca-5dd9567d15a6
last-substantial-update: 2022-10-01T00:00:00Z
thumbnail: KT-781.jpg
duration: 405
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 0%

---

# 了解Adobe Managed Services上使用AEM进行Adobe IMS身份验证{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager引入了Admin Console对AEM实例的支持，并针对Managed Services上的Adobe Identity Management System (IMS)的身份验证引入了AEM支持。   此集成允许AEM Managed Services客户在单个统一的Web控制台中管理所有Experience Cloud用户。 可以将用户和组分配到与AEM实例关联的产品配置文件，从而授予对特定AEM实例的集中管理访问权限。

>[!VIDEO](https://video.tv.adobe.com/v/327598?quality=12&learn=on&captions=chi_hans)

* Adobe Experience Manager IMS身份验证支持仅针对“内部”用户（作者、查看者、管理员、开发人员等），不适用于网站访客等外部最终用户。
* [Admin Console](https://adminconsole.adobe.com/)将AEM Managed Services客户表示为IMS组织，将AEM实例表示为产品上下文。 Admin Console系统和产品管理员可以定义和管理。
* AEM Managed Services将您的拓扑与Admin Console同步，在产品上下文和AEM实例之间创建一对一映射。
* Admin Console中的产品配置文件决定用户可以访问哪些AEM实例。
* 身份验证支持包括用于SSO的符合SAML2标准的客户IDP。
* 仅支持Enterprise ID或Federated ID（适用于客户SSO）(不支持个人Adobe ID)。

*&#42;Adobe Managed Services客户的AEM 6.4 SP3及更高版本支持此功能。*

## 最佳实践 {#best-practices}

### 在Admin Console中应用权限

应避免在Admin Console和Adobe Experience Manager中在用户级别应用权限和访问权限。

在Admin Console中，应通过产品上下文级别的用户组向用户授予访问权限。 用户组通常最好通过组织内的逻辑角色来表示，以提高组在Adobe Experience Cloud产品中的可重用性。

### 在Adobe Experience Manager中应用权限

在Adobe Experience Manager中，与Adobe IMS同步的用户组应定期添加到[AEM提供的用户组](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=zh-Hans)，这些用户组已预配置相应权限，以执行AEM中的特定任务集。 从Adobe IMS同步的用户不应直接添加到[AEM提供的用户组](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=zh-Hans)。
