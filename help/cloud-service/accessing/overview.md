---
title: 将对AEM的访问配置为Cloud Service
description: 'AEM as a A A A A A Target是利用AEM应用程序的云原生方式，因此，可利用AdobeIMS(Identity Management系统)来促进管理员和普通用户登录到AEM创作服务。 了解如何将AdobeIMS用户、用户组和产品配置文件与AEM组和权限结合使用，以提供对AEM作者的特定访问权限。  '
version: cloud-service
topic: 管理、安全
feature: 用户和群组
role: Admin
level: Beginner
kt: 5882
thumbnail: KT-5882.jpg
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 0%

---


# 将对AEM的访问配置为Cloud Service

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="AdobeIMS简介"
>abstract="AEM as aCloud Service可利用AdobeIMS(Identity Management系统)来促进管理员和普通用户登录到AEM创作服务。 了解如何将AdobeIMS用户、组和产品配置文件与AEM组和权限一起使用，以提供对AEM创作服务的细粒度访问。"

AEM as a A A A A A Service是利用AEM应用程序的云原生方式，因此，可利用AdobeIMS(Identity Management系统)促进其管理员和普通用户登录AEM创作服务。

![Adobe Admin Console](./assets/hero.png)

了解如何将AdobeIMS用户、组和产品配置文件与AEM组和权限一起使用，以提供对AEM创作服务的细粒度访问。

## AdobeIMS用户

需要访问AEM创作服务的用户在[Adobe的AdminConsole](https://adminconsole.adobe.com)中作为[AdobeIMS用户](https://helpx.adobe.com/cn/enterprise/using/set-up-identity.html)进行管理。 了解IMS用户是什么Adobe，以及如何在Admin Console中访问和管理他们。

[了解AdobeIMS用户](./adobe-ims-users.md)

## AdobeIMS用户组

访问AEM创作服务的用户应使用[[Adobe的AdminConsole](https://adminconsole.adobe.com)中的AdobeIMS用户组](https://helpx.adobe.com/enterprise/using/user-groups.html)组织为逻辑组。 AdobeIMS用户组不提供对AEM的直接权限或访问权限(这是[AdobeIMS产品配置文件](#adobe-ims-product-profiles)的作业)，但是，它们是定义用户的逻辑分组的绝佳方式，这些逻辑分组又可以通过AEM组和权限转换为AEM创作服务中的特定访问级别。

[了解AdobeIMS用户组](./adobe-ims-user-groups.md)

## AdobeIMS产品配置文件

[AdobeIMS产品配置文件](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html)(在Adobe的 [AdminConsole中管理](https://adminconsole.adobe.com))是提供 [AdobeIMS用](#adobe-ims-users) 户访问权限以通过基本级别访问登录到AEM创作服务的机械工。

+ __AEM Users__&#x200B;产品配置文件为用户提供了AEM Contributors组中的成员资格，以只读方式访问AEM。
+ __AEM管理员__&#x200B;产品配置文件为用户提供了对AEM的完全管理访问权限。

[了解AdobeIMS产品配置文件](./adobe-ims-product-profiles.md)

## AEM用户组和权限

Adobe Experience Manager以AdobeIMS用户、用户组和产品配置文件为基础，旨在为用户提供对AEM的可自定义的访问权限。 了解如何构建AEM组和权限，以及它们如何与AdobeIMS抽象概念协同工作，以提供对AEM的无缝、可自定义的访问。

[了解AEM用户、组和权限](./aem-users-groups-and-permissions.md)

## 访问和权限演练

一个简略的演练，用于在AdobeAdminConsole中配置AdobeIMS用户、用户组和产品配置文件，以及如何利用AEM Author中的这些AdobeIMS抽象概念来定义和管理特定的基于组的权限。

[AEM访问和权限演练](./walk-through.md)

## 其他Adobe Admin Console资源

以下文档涵盖[Adobe Admin Console](https://adminconsole.adobe.com)特定的详细信息和关注事项，这些信息和关注事项可能有助于更好地了解Adobe Admin Console并使用它来管理用户和跨Experience Cloud产品的访问。

+ [Adobe Admin Console Identity概述](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Adobe Admin Console管理员角色](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Adobe Admin Console开发人员角色](https://helpx.adobe.com/enterprise/using/manage-developers.html)