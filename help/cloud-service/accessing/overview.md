---
title: 配置对 AEM as a Cloud Service 的访问权限
description: AEM as a Cloud Service是利用AEM应用程序的云原生方式，因此，会利用Adobe IMS (Identity Management System)帮助管理员和常规用户都登录到AEM Author服务。 了解Adobe IMS用户、用户组和产品配置文件如何与AEM组和权限结合使用，以提供对AEM Author的特定访问。
version: Experience Manager as a Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
jira: KT-5882
thumbnail: KT-5882.jpg
last-substantial-update: 2022-10-06T00:00:00Z
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
duration: 113
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 26%

---

# 配置对 AEM as a Cloud Service 的访问权限 {#configuring-access-to-aem-as-a-cloud-service}

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Adobe IMS 简介"
>abstract="AEM as a Cloud Service 利用 Adobe IMS (Identity Management System) 推动其用户（包括管理员和普通用户）登录 AEM Author 服务。了解 Adobe IMS 用户、组和产品配置文件如何与 AEM 组和权限结合使用，以提供对 AEM Author 服务的精细访问。"

AEM as a Cloud Service是利用AEM应用程序的云原生方式，因此，会利用Adobe IMS (Identity Management System)为其用户（管理员和常规用户）登录AEM Author服务提供便利。

![Adobe Admin Console](./assets/hero.png)

了解 Adobe IMS 用户、组和产品配置文件如何与 AEM 组和权限结合使用，以提供对 AEM Author 服务的精细访问。

## Adobe IMS 用户

在[Adobe的AdminConsole](https://adminconsole.adobe.com)中，需要访问AEM创作服务的用户被管理为[Adobe IMS用户](https://helpx.adobe.com/cn/enterprise/using/set-up-identity.html)。 了解 Adobe IMS 用户的身份，以及如何在 Admin Console 中访问和管理他们。

>[!NOTE]
>
>从AdminConsole中删除IMS用户后，该用户不会自动从AEM中删除，但是AEM会话（令牌）一旦过期，便无法登录到AEM。


[了解Adobe IMS用户](./adobe-ims-users.md)

## Adobe IMS 用户组

应该在[Adobe的AdminConsole](https://adminconsole.adobe.com)中使用[Adobe IMS用户组](https://helpx.adobe.com/cn/enterprise/using/user-groups.html)将访问AEM创作服务的用户组织到逻辑组中。 Adobe IMS用户组不提供对AEM的直接权限或访问权限（这是[Adobe IMS产品配置文件](#adobe-ims-product-profiles)的作业），但是，它们非常适合定义用户的逻辑分组，进而可以使用AEM组和权限在AEM Author服务中转换为特定级别的访问权限。

[了解Adobe IMS用户组](./adobe-ims-user-groups.md)

## Adobe IMS 产品配置文件

[&#128279;](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html)在[Adobe的AdminConsole](https://adminconsole.adobe.com)中管理的Adobe IMS产品配置文件是为[Adobe IMS用户](#adobe-ims-users)提供基本访问权限以登录AEM创作服务的访问权限的机制。

+ __AEM Users__&#x200B;产品配置文件为用户提供了对AEM的只读访问权限，这些用户可通过AEM Contributors组的成员资格进行访问。
+ __AEM管理员__&#x200B;产品配置文件为用户提供了对AEM的完全管理访问权限。

[了解Adobe IMS产品配置文件](./adobe-ims-product-profiles.md)

## AEM用户组和权限

Adobe Experience Manager 基于 Adobe IMS 用户、用户组和产品配置文件进行构建，以便向用户提供对 AEM 的可定制的访问权限。了解如何构建AEM组和权限，以及它们如何与Adobe IMS抽象概念协同工作，从而提供对AEM的无缝访问和可自定义的访问。

[了解AEM用户、组和权限](./aem-users-groups-and-permissions.md)

## 访问和权限演练

概要介绍如何在Adobe AdminConsole中配置Adobe IMS用户、用户组和产品配置文件，以及如何利用AEM Author中的这些Adobe IMS抽象概念来定义和管理特定的基于组的权限。

[AEM访问权限演练](./walk-through.md)

## 其他Adobe Admin Console资源

以下文档涵盖了[Adobe Admin Console](https://adminconsole.adobe.com)特定的详细信息和问题，这些详细信息和问题可能有助于更好地了解Adobe Admin Console，并可用于跨Experience Cloud产品管理用户和访问权限。

+ [Adobe Admin Console标识概述](https://helpx.adobe.com/cn/enterprise/using/identity.html)
+ [Adobe Admin Console管理员角色](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Adobe Admin Console开发人员角色](https://helpx.adobe.com/enterprise/using/manage-developers.html)
