---
title: 配置 AEM as a Cloud Service 的访问权限
description: AEMas a Cloud Service是雲端原生方式來利用AEM應用程式，因此會利用Adobe IMS (Identity Management系統)來協助使用者（管理員和一般使用者）登入AEM Author服務。 瞭解Adobe IMS使用者、使用者群組和產品設定檔如何結合AEM群組和許可權使用，以提供AEM Author的特定存取權。
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

AEMas a Cloud Service是雲端原生方式來利用AEM應用程式，因此會利用Adobe IMS (Identity Management系統)來協助其使用者（管理員和一般使用者）登入AEM Author服務。

![Adobe Admin Console](./assets/hero.png)

了解 Adobe IMS 用户、组和产品配置文件如何与 AEM 组和权限结合使用，以提供对 AEM Author 服务的精细访问。

## Adobe IMS使用者

需要存取AEM作者服務的使用者管理為 [Adobe IMS使用者](https://helpx.adobe.com/cn/enterprise/using/set-up-identity.html) 在 [Adobe的AdminConsole](https://adminconsole.adobe.com). 了解 Adobe IMS 用户的身份，以及如何在 Admin Console 中访问和管理他们。

>[!NOTE]
>
>從AdminConsole刪除IMS使用者時，不會自動從AEM刪除，但AEM工作階段（代號）過期後，就無法登入AEM。


[瞭解Adobe IMS使用者](./adobe-ims-users.md)

## Adobe IMS使用者群組

應該使用將存取AEM作者服務的使用者組織到邏輯群組中 [Adobe IMS使用者群組](https://helpx.adobe.com/cn/enterprise/using/user-groups.html) 在 [Adobe的AdminConsole](https://adminconsole.adobe.com). Adobe IMS使用者群組不提供直接許可權或AEM存取權(這是以下工作： [Adobe IMS產品設定檔](#adobe-ims-product-profiles))但是，它們是定義使用者的邏輯分組的絕佳方式，進而可以使用AEM群組和許可權，轉換為AEM Author服務中的特定存取層級。

[瞭解Adobe IMS使用者群組](./adobe-ims-user-groups.md)

## Adobe IMS產品設定檔

[Adobe IMS產品設定檔](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html)，管理位置 [Adobe的AdminConsole](https://adminconsole.adobe.com)，是提供 [Adobe IMS使用者](#adobe-ims-users) 以基本存取層級登入AEM Author服務的存取權。

+ 此 __AEM使用者__ 產品設定檔可讓使用者透過AEM貢獻者群組的成員資格，以唯讀方式存取AEM。
+ 此 __AEM管理員__ 產品設定檔為使用者提供AEM的完整管理存取權。

[瞭解Adobe IMS產品設定檔](./adobe-ims-product-profiles.md)

## AEM使用者群組與許可權

Adobe Experience Manager 基于 Adobe IMS 用户、用户组和产品配置文件进行构建，以便向用户提供对 AEM 的可定制的访问权限。瞭解如何建立AEM群組和許可權，以及它們如何與Adobe IMS抽象化功能搭配運作，以提供順暢且可自訂的AEM存取權。

[瞭解AEM使用者、群組和許可權](./aem-users-groups-and-permissions.md)

## 存取和許可權逐步說明

在AdobeAdminConsole中設定Adobe IMS使用者、使用者群組和產品設定檔的簡略逐步解說，以及如何在AEM Author中運用這些Adobe IMS抽象化功能來定義和管理特定的群組型許可權。

[AEM存取和許可權逐步說明](./walk-through.md)

## 其他Adobe Admin Console資源

下列檔案封面 [Adobe Admin Console](https://adminconsole.adobe.com) — 特定的詳細資訊和顧慮，可能有助於更好地瞭解Adobe Admin Console，並使用它來管理使用者和跨Experience Cloud產品的存取權。

+ [Adobe Admin Console 标识概述](https://helpx.adobe.com/cn/enterprise/using/identity.html)
+ [Adobe Admin Console管理員角色](https://helpx.adobe.com/enterprise/using/admin-roles.html)
+ [Adobe Admin Console開發人員角色](https://helpx.adobe.com/enterprise/using/manage-developers.html)
