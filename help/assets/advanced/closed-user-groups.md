---
title: AEM Assets中已关闭的用户组
description: “已关闭的用户组”(CUG)是一项功能，用于限制对已发布站点上的选定用户组的内容访问权限。 此视频显示了如何将已关闭的用户组与Adobe Experience Manager资产一起使用，以限制对特定资产文件夹的访问权限。
version: 6.3, 6.4, 6.5, cloud-service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
kt: 649
thumbnail: 22155.jpg
translation-type: tm+mt
source-git-commit: 407840a0e0c90c4f004390a052d036f9b69fa8df
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 1%

---


# 已关闭的用户组{#using-closed-user-groups-with-aem-assets}

“已关闭的用户组”(CUG)是一项功能，用于限制对已发布站点上的选定用户组的内容访问权限。 此视频显示了如何将已关闭的用户组与Adobe Experience Manager资产一起使用，以限制对特定资产文件夹的访问权限。 AEM 6.4中首次引入了对AEM Assets封闭用户组的支持。

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## 已关闭的用户组(CUG)(带AEM Assets)

* 旨在限制对AEM Publish实例上的资产的访问。
* 授予对一组用户/组的读取权限。
* CUG只能在文件夹级别进行配置。 无法对单个资产设置CUG。
* CUG策略由任何子文件夹和已应用的资产自动继承。
* 通过设置新的CUG策略，CUG策略可以被子文件夹覆盖。 这应少用，不应被视为最佳做法。

## 已关闭的用户组与访问控制列表{#closed-user-groups-vs-access-control-lists}

已关闭的用户组(CUG)和访问控制列表(ACL)用于控制对AEM中内容的访问，并基于AEM Security用户和组。 然而，这些功能的应用和实现却大不相同。 下表总结了两种特征之间的区别。

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| 预期用途 | 为&#x200B;**current** AEM实例上的内容配置和应用权限。 | 为AEM **author**&#x200B;实例上的内容配置CUG策略。 对AEM **publish**&#x200B;实例上的内容应用CUG策略。 |
| 权限级别 | 定义所有级别的用户/组的已授权/已拒绝权限：读取、修改、创建、删除、读取ACL、编辑ACL、复制。 | 授予对一组用户/组的读取权限。 拒绝对&#x200B;*所有其他*&#x200B;用户/组的读取访问。 |
| 发布 | ACL是&#x200B;*不*&#x200B;随内容发布的。 | CUG策略&#x200B;*是与内容一起发布的*。 |

## 支持链接{#supporting-links}

* [管理资产和已关闭的用户组](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [创建已关闭的用户组](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Oak已关闭的用户组文档](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
