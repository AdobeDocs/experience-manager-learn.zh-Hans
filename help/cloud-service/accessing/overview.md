---
title: 配置对 AEM as a Cloud Service 的访问权限
description: AEM as a Cloud Service 是利用 AEM 应用程序的云原生方式，因此，它利用 Adobe IMS（身份管理系统）来方便用户（包括管理员和普通用户）登录 AEM Author 服务。了解 Adobe IMS 用户、用户组和产品轮廓如何与 AEM 群组和权限结合使用，以提供对 AEM Author 的特定访问权限。
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
workflow-type: ht
source-wordcount: '598'
ht-degree: 100%

---

# 配置对 AEM as a Cloud Service 的访问权限 {#configuring-access-to-aem-as-a-cloud-service}

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Adobe IMS 简介"
>abstract="AEM as a Cloud Service 利用 Adobe IMS (Identity Management System) 推动其用户（包括管理员和普通用户）登录 AEM Author 服务。了解 Adobe IMS 用户、组和产品轮廓如何与 AEM 组和权限结合使用，以提供对 AEM Author 服务的精细访问。"

AEM as a Cloud Service 是利用 AEM 应用程序的云原生方式，因此，它利用 Adobe IMS（身份管理系统）来方便用户（包括管理员和普通用户）登录 AEM Author 服务。

![Adobe Admin Console](./assets/hero.png)

了解 Adobe IMS 用户、群组和产品轮廓如何与 AEM 群组和权限结合使用，以提供对 AEM Author 服务的精细访问。

## Adobe IMS 用户

需要访问 AEM Author 服务的用户在 [Adobe 的 AdminConsole](https://adminconsole.adobe.com) 中作为 [Adobe IMS 用户](https://helpx.adobe.com/cn/enterprise/using/set-up-identity.html)进行管理。了解 Adobe IMS 用户的定义，以及如何通过 Admin Console 访问和管理这些用户。

>[!NOTE]
>
>当从 AdminConsole 中删除 IMS 用户时，该用户并不会自动从 AEM 中删除，但一旦 AEM 会话（令牌）过期，他们将无法登录 AEM。


[了解 Adobe IMS 用户](./adobe-ims-users.md)

## Adobe IMS 用户组

应使用 [Adobe 的 AdminConsole](https://adminconsole.adobe.com) 中的 [Adobe IMS 用户组](https://helpx.adobe.com/cn/enterprise/using/user-groups.html)，将访问 AEM Author 服务的用户组织成逻辑组。尽管 Adobe IMS 用户组本身并不直接赋予对 AEM 的权限或访问（此功能由 [Adobe IMS 产品轮廓](#adobe-ims-product-profiles)负责），但它们可用于定义用户的逻辑分组。通过将这些 IMS 用户组与 AEM 中的用户组和权限相结合，可将其映射为 AEM Author 服务中的特定访问级别。

[了解 Adobe IMS 用户组](./adobe-ims-user-groups.md)

## Adobe IMS 产品轮廓

在 [Adobe 的 AdminConsole](https://adminconsole.adobe.com) 中进行管理的 [Adobe IMS 产品轮廓](https://helpx.adobe.com/cn/enterprise/using/manage-permissions-and-roles.html)是 [Adobe IMS 用户](#adobe-ims-users)登录 AEM Author 服务并获得基本访问权限的机制。

+ __AEM 用户__&#x200B;产品轮廓通过将用户加入 AEM 的“投稿人”组，为其提供对 AEM 的只读访问权限。
+ __AEM 管理员__&#x200B;产品轮廓为用户提供对 AEM 的完整管理员访问权限。

[了解有关 Adobe IMS 产品轮廓的信息](./adobe-ims-product-profiles.md)

## AEM 用户组和权限

Adobe Experience Manager 基于 Adobe IMS 用户、用户组和产品轮廓进行构建，以便为用户提供可自定义的 AEM 访问权限。学习如何构建 AEM 用户组和权限，以及它们如何与 Adobe IMS 抽象概念协同工作，以实现无缝且可自定义的 AEM 访问控制。

[了解 AEM 用户、用户组及权限设置](./aem-users-groups-and-permissions.md)

## 访问与权限演练

一个简要的演练，展示如何在 Adobe AdminConsole 中配置 Adobe IMS 用户、用户组和产品轮廓，以及如何在 AEM Author 中利用这些 IMS 抽象来定义和管理基于组的特定权限。

[AEM 访问与权限演练](./walk-through.md)

## 其他 Adobe Admin Console 资源

以下文档涵盖了与 [Adobe Admin Console](https://adminconsole.adobe.com) 相关的详细信息和注意事项，可帮助您更好地理解 Adobe Admin Console 并利用其管理 Experience Cloud 产品的用户及访问权限。

+ [Adobe Admin Console 身份标识概述](https://helpx.adobe.com/cn/enterprise/using/identity.html)
+ [Adobe Admin Console 管理员角色](https://helpx.adobe.com/cn/enterprise/using/admin-roles.html)
+ [Adobe Admin Console 开发人员角色](https://helpx.adobe.com/cn/enterprise/using/manage-developers.html)
