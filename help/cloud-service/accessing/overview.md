---
title: 将对AEM的访问配置为Cloud Service
description: 'AEM作为Cloud Service是利用AEM应用程序的云本机方式，因此，利用AdobeIMS(Identity Management系统)方便管理员和普通用户登录到AEM作者服务。 了解AdobeIMS用户、用户组和产品用户档案如何与AEM组和权限一起使用，以提供对AEM作者的特定访问权限。  '
feature: users-and-groups
topics: authentication
version: cloud-service
activity: setup
audience: administrator
doc-type: article
kt: 5882
thumbnail: KT-5882.jpg
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '537'
ht-degree: 0%

---


# 将对AEM的访问配置为Cloud Service

AEM作为Cloud Service是利用AEM应用程序的云本机方式，因此，它利用AdobeIMS(Identity Management系统)来促进其用户（管理员和普通用户）登录AEM作者服务。 了解如何与AEM组一起使用AdobeIMS用户、组和产品用户档案，以及如何通过权限提供对AEM作者服务的细致访问。

## AdobeIMS用户

需要访问AEM作者服务的用户在Adobe的 [AdminConsole中作为Adobe](https://helpx.adobe.com/cn/enterprise/using/set-up-identity.html) IMS [用户进行管理](https://adminconsole.adobe.com)。 了解IMSAdobe的用户，以及如何在Admin Console中访问和管理他们。

[了解AdobeIMS用户](./adobe-ims-users.md)

## AdobeIMS用户组

访问AEM作者服务的用户应使用Adobe的AdminConsole [中的AdobeIMS用户组](https://helpx.adobe.com/enterprise/using/user-groups.html) ，将 [其组织到逻辑组中](https://adminconsole.adobe.com)。 AdobeIMS用户组不提供直接权限或对AEM的访问(这是 [AdobeIMS产品用户档案的工作](#adobe-ims-product-profiles))，但是，它们是定义逻辑用户组(这些用户组反过来可以使用AEM组和权限转换为AEM作者服务中的特定访问级别)的极好方法。

[了解AdobeIMS用户组](./adobe-ims-user-groups.md)

## AdobeIMS产品用户档案

[AdobeIMS产品用户档案](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html)(在 [Adobe的AdminConsole中进行管理](https://adminconsole.adobe.com))是一种机械手，它为 [AdobeIMS用户提供访问权限](#adobe-ims-users) ，以基本级别访问登录AEM作者服务。

+ AEM __Users产品用户档案__ ，用户可通过AEM Compiters组中的成员身份以只读方式访问AEM。
+ AEM Administrators __产品用户档案__ ，为用户提供对AEM的完全、管理访问权限。

[了解AdobeIMS产品用户档案](./adobe-ims-product-profiles.md)

## AEM用户组和权限

Adobe Experience Manager以AdobeIMS用户、用户组和产品用户档案为基础，为用户提供对AEM的可自定义访问。 了解如何构建AEM组和权限，以及它们如何与AdobeIMS抽象协同工作，以提供对AEM的无缝、可自定义的访问。

[了解AEM用户、组和权限](./aem-users-groups-and-permissions.md)

## 访问和权限浏览

简略演练：在AdobeAdminConsole中配置AdobeIMS用户、用户组和产品用户档案，以及如何利用AEM作者中的这些AdobeIMS抽象定义和管理基于特定组的权限。

[AEM访问和权限浏览](./walk-through.md)

## Adobe Admin Console其他资源

以下文档涵盖 [Adobe Admin Console特](https://adminconsole.adobe.com)定的详细信息和疑虑，这些细节和疑虑可能有助于更好地了解Adobe Admin Console并利用管理用户和跨Experience Cloud产品访问。

+ [Adobe Admin Console身份概述](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Adobe Admin Console管理员角色](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Adobe Admin Console开发人员角色](https://helpx.adobe.com/enterprise/using/manage-developers.html)