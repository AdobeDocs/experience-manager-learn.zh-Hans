---
title: 配置 AEM as a Cloud Service 的访问权限
description: AEMas a Cloud Service是利用AEM应用程序的云原生方式，因此，会利用Adobe IMS(Identity Management系统)来促进管理员和普通用户用户登录到AEM创作服务。 了解如何将Adobe IMS用户、用户组和产品配置文件与AEM组和权限结合使用，以提供对AEM作者的特定访问权限。
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
kt: 5882
thumbnail: KT-5882.jpg
last-substantial-update: 2022-10-06T00:00:00Z
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
source-git-commit: 4c91ab68f6e31f0eb549689c7ecfd0ee009801d9
workflow-type: tm+mt
source-wordcount: '626'
ht-degree: 26%

---

# 配置 AEM as a Cloud Service 的访问权限 {#configuring-access-to-aem-as-a-cloud-service}

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Adobe IMS 简介"
>abstract="AEM as a Cloud Service 利用 Adobe IMS (Identity Management System) 推动其用户（包括管理员和普通用户）登录 AEM Author 服务。了解 Adobe IMS 用户、组和产品配置文件如何与 AEM 组和权限结合使用，以提供对 AEM Author 服务的精细访问。"

AEMas a Cloud Service是利用AEM应用程序的云原生方式，因此，会利用Adobe IMS(Identity Management系统)来促进管理员和普通用户的用户登录到AEM创作服务。

![Adobe Admin Console](./assets/hero.png)

了解 Adobe IMS 用户、组和产品配置文件如何与 AEM 组和权限结合使用，以提供对 AEM Author 服务的精细访问。

## Adobe IMS用户

需要访问AEM创作服务的用户将作为 [Adobe IMS用户](https://helpx.adobe.com/cn/enterprise/using/set-up-identity.html) in [Adobe的AdminConsole](https://adminconsole.adobe.com). 了解 Adobe IMS 用户的身份，以及如何在 Admin Console 中访问和管理他们。

>[!NOTE]
>
>从AEM Console中删除IMS用户后，该用户不会自动从IMS中删除，但是，当AEM会话（令牌）过期后，他们将无法登录到IMS。


[了解Adobe IMS用户](./adobe-ims-users.md)

## Adobe IMS用户组

访问AEM创作服务的用户应使用 [Adobe IMS用户组](https://helpx.adobe.com/cn/enterprise/using/user-groups.html) in [Adobe的AdminConsole](https://adminconsole.adobe.com). Adobe IMS用户组不提供对AEM的直接权限或访问权限(这是 [Adobe IMS产品配置文件](#adobe-ims-product-profiles))，但是，它们是定义用户逻辑分组的绝佳方式，这些逻辑分组反过来也可以通过使用AEM组和权限转换到AEM创作服务中的特定访问级别。

[了解Adobe IMS用户组](./adobe-ims-user-groups.md)

## Adobe IMS产品配置文件

[Adobe IMS产品配置文件](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html)，在中管理 [Adobe的AdminConsole](https://adminconsole.adobe.com)，是提供 [Adobe IMS用户](#adobe-ims-users) 具有登录AEM创作服务的基本访问权限。

+ 的 __AEM用户__ 产品配置文件为用户提供了通过AEM参与者组中的成员资格进行AEM的只读访问权限。
+ 的 __AEM管理员__ 产品配置文件为用户提供了对AEM的完全、管理性访问权限。

[了解Adobe IMS产品配置文件](./adobe-ims-product-profiles.md)

## AEM用户组和权限

Adobe Experience Manager 基于 Adobe IMS 用户、用户组和产品配置文件进行构建，以便向用户提供对 AEM 的可定制的访问权限。了解如何构建AEM组和权限，以及它们如何与Adobe IMS抽象概念协同工作，以提供对AEM的无缝、可自定义的访问。

[了解AEM用户、组和权限](./aem-users-groups-and-permissions.md)

## 访问和权限演练

一个简略的演练，用于在AdminConsole中配置Adobe IMS用户、用户组和产品配置文件，以及如何利用AEM Author中的这些Adobe IMS抽象概念来定义和管理特定的基于组的权限。

[AEM访问和权限演练](./walk-through.md)

## 其他Adobe Admin Console资源

以下文档介绍 [Adobe Admin Console](https://adminconsole.adobe.com)特定的详细信息和关注事项，可能有助于更好地了解Adobe Admin Console并使用它来管理用户和跨Experience Cloud产品的访问。

+ [Adobe Admin Console 标识概述](https://helpx.adobe.com/cn/enterprise/using/identity.html)
+ [Adobe Admin Console管理员角色](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Adobe Admin Console开发人员角色](https://helpx.adobe.com/enterprise/using/manage-developers.html)
