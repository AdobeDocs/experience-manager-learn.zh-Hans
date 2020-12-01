---
title: 了解AdobeIMS身份验证与AEM在Adobe Managed Services上的协作
description: Adobe Experience Manager推出Admin Console支持AEM实例和AdobeIMS(Identity Management系统)，以验证AEM在Managed Services的身份。   此集成允许AEMManaged Services客户在一个统一的Web控制台中管理所有Experience Cloud用户。 可以将用户和用户组分配给与AEM实例关联的产品用户档案，从而授予对特定AEM实例的集中管理访问权。
version: 6.4, 6.5
feature: authentication
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 0%

---


# 了解AdobeIMS身份验证与AEM在Adobe Managed Services上的协作{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager推出Admin Console支持AEM实例和AdobeIMS(Identity Management系统)，以验证AEM在Managed Services的身份。   此集成允许AEMManaged Services客户在一个统一的Web控制台中管理所有Experience Cloud用户。 可以将用户和用户组分配给与AEM实例关联的产品用户档案，从而授予对特定AEM实例的集中管理访问权。

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Adobe Experience ManagerIMS身份验证支持仅针对“内部”用户（作者、审阅者、管理员、开发人员等），而不针对外部最终用户(如网站访客)。
* [Admin Console](https://adminconsole.adobe.com/) 将AEMManaged Services客户作为IMS组织，AEM实例作为产品上下文。Admin Console系统和产品管理员可以定义和管理。
* AEMManaged Services将拓扑与Admin Console同步，在产品上下文和AEM实例之间创建1对1映射。
* Admin Console中的产品用户档案将决定用户可以访问的AEM实例。
* 身份验证支持包括符合客户SAML2的IDP，用于SSO。
* 将仅支持Enterprise ID或Federated ID（对于客户SSO）(不支持个人AdobeID)。

**AEM 6.4 SP3及更高版本支持此功能，适用于Adobe Managed Services客户。*

## 最佳实践{#best-practices}

### 在Admin Console中应用权限

在Admin Console和Adobe Experience Manager应避免在用户级别应用权限和访问。

在Admin Console中，应通过Product Context级别的用户组授予用户访问权限。 用户组通常以组织内的逻辑角色来最好地表示，以促进组在Adobe Experience Cloud产品中的可重用性。

>[!NOTE]
>
> 如果使用AEM作为Cloud Service，则直接将Admin Console用户分配给产品用户档案。 对于AEM aAdmin Console，不支持通过Admin Console用户组在Cloud Service用户与产品用户档案之间进行传递权限。

### 在Adobe Experience Manager应用权限

在Adobe Experience Manager，从AdobeIMS同步的用户组应以术语添加到[AEM提供的用户组](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html)，后者预配置了在AEM中执行特定任务集的适当权限。 从AdobeIMS同步的用户不应直接添加到AEM提供的用户组[。](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html)
