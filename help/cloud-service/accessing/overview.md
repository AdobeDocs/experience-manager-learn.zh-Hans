---
title: 将对AEM的访问配置为Cloud Service
description: 'AEM作为Cloud Service是利用AEM应用程序的云本机方式，因此，利用Adobe IMS(Identity Management系统)方便管理员和普通用户登录到AEM作者服务。 了解Adobe IMS用户、用户组和产品用户档案如何与AEM组和权限一起使用，以提供对AEM作者的特定访问。  '
feature: users-and-groups
topics: authentication
version: cloud-service
activity: setup
audience: administrator
doc-type: article
kt: 5882
thumbnail: KT-5882.jpg
translation-type: tm+mt
source-git-commit: f30d15f0578b7e529e4acefb8e1d2e29157ab359
workflow-type: tm+mt
source-wordcount: '595'
ht-degree: 0%

---


# 将对AEM的访问配置为Cloud Service

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Adobe IMS简介"
>abstract="AEM作为Cloud Service，利用Adobe IMS(Identity Management System)，方便管理员和普通用户登录到AEM作者服务。 了解Adobe IMS用户、组和产品用户档案如何与AEM组和权限一起使用，以提供对AEM创作服务的细致访问。"

AEM作为Cloud Service是利用AEM应用程序的云本机方式，因此，它利用Adobe IMS(Identity Management系统)来方便其用户（管理员和普通用户）登录到AEM作者服务。 了解Adobe IMS用户、组和产品用户档案如何与AEM组和权限一起使用，以提供对AEM创作服务的细致访问。

## AdobeIMS用户

需要访问AEM作者服务的用户在[Adobe的AdminConsole](https://adminconsole.adobe.com)中作为[AdobeIMS用户](https://helpx.adobe.com/cn/enterprise/using/set-up-identity.html)进行管理。 了解哪些Adobe IMS用户以及如何在Admin Console中访问和管理他们。

[了解Adobe IMS用户](./adobe-ims-users.md)

## AdobeIMS用户组

使用[Adobe的AdminConsole](https://adminconsole.adobe.com)中的[AdobeIMS用户组](https://helpx.adobe.com/enterprise/using/user-groups.html)，访问AEM作者服务的用户应组织到逻辑组中。 Adobe IMS用户组不提供对AEM的直接权限或访问权限(这是[AdobeIMS产品用户档案](#adobe-ims-product-profiles)的作业)，但是，使用AEM组和权限，它们是定义用户的逻辑组（这些用户组反过来可以转换为AEM作者服务中的特定访问级别）的一种极好方法。

[了解Adobe IMS用户组](./adobe-ims-user-groups.md)

## AdobeIMS产品用户档案

[Adobe IMS产品用户档案](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html)(在Adobe [的AdminConsole中管理](https://adminconsole.adobe.com))是提供 [Adobe IMS用](#adobe-ims-users) 户访问权限以基本访问级别登录AEM Author服务的机制。

+ __AEM Users__&#x200B;产品用户档案允许用户通过AEM Contributors组中的成员身份对AEM进行只读访问。
+ __AEM Administrators__&#x200B;产品用户档案为用户提供对AEM的完全、管理访问。

[了解Adobe IMS产品用户档案](./adobe-ims-product-profiles.md)

## AEM用户组和权限

Adobe Experience Manager以Adobe IMS用户、用户组和产品用户档案为构建基础，为用户提供对AEM的可自定义访问。 了解如何构建AEM组和权限，以及它们如何与Adobe IMS抽象协同工作，以提供对AEM的无缝、可自定义的访问。

[了解AEM用户、用户组和权限](./aem-users-groups-and-permissions.md)

## 访问和权限浏览

一个简略的演练，介绍如何在Adobe AdminConsole中配置Adobe IMS用户、用户组和产品用户档案，以及如何利用AEM作者中的这些AdobeIMS抽象来定义和管理特定基于组的权限。

[AEM访问和权限漫游](./walk-through.md)

## 其他Adobe Admin Console资源

以下文档涵盖[Adobe Admin Console](https://adminconsole.adobe.com)特定的详细信息和关注事项，有助于更好地了解Adobe Admin Console并使用它管理用户和跨Experience Cloud产品访问。

+ [Adobe Admin Console Identity概述](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Adobe Admin Console管理员角色](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Adobe Admin Console开发人员角色](https://helpx.adobe.com/enterprise/using/manage-developers.html)