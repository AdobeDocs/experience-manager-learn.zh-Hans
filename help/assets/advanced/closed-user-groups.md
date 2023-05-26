---
title: AEM Assets中的封闭用户组
description: 封闭用户组(CUG)是一项功能，用于限制对已发布网站上一组选定用户的内容访问。 本视频说明封闭用户组如何与Adobe Experience Manager Assets一起使用，以限制对资源的特定文件夹的访问。
version: 6.4, 6.5, Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
kt: 649
thumbnail: 22155.jpg
last-substantial-update: 2022-06-06T00:00:00Z
exl-id: a2bf8a82-15ee-478c-b7c3-de8a991dfeb8
source-git-commit: f37483f90f2a707c906e1e206795fdebb5f698e9
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 0%

---

# 封闭用户组{#using-closed-user-groups-with-aem-assets}

封闭用户组(CUG)是一项功能，用于限制对已发布网站上一组选定用户的内容访问。 本视频说明封闭用户组如何与Adobe Experience Manager Assets一起使用，以限制对资源的特定文件夹的访问。 AEM 6.4中首次引入了AEM Assets对封闭用户组的支持。

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## AEM Assets的封闭用户组(CUG)

* 旨在限制对AEM发布实例上资产的访问。
* 授予对一组用户/组的读取权限。
* CUG只能在文件夹级别配置。 无法在单个资产上设置CUG。
* 任何子文件夹和应用的资产都会自动继承CUG策略。
* 通过设置新的CUG策略，子文件夹可以覆盖CUG策略。 应谨慎使用，这不应被视为最佳实践。

## 封闭用户组与访问控制列表 {#closed-user-groups-vs-access-control-lists}

封闭用户组(CUG)和访问控制列表(ACL)均可用于根据AEM Security用户和组控制对AEM中内容的访问。 然而，这些功能的应用和实施方式却大相径庭。 下表总结了这两项功能之间的区别。

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| 目标用途 | 为上的内容配置和应用权限 **当前** AEM实例。 | 为AEM上的内容配置CUG策略 **作者** 实例。 对AEM上的内容应用CUG策略 **发布** 实例。 |
| 权限级别 | 为用户/组定义所有级别的授予/拒绝权限：读取、修改、创建、删除、读取ACL、编辑ACL、复制。 | 授予对一组用户/组的读取权限。 拒绝对的读取权限 *所有其他* 用户/组。 |
| 发布 | ACL是 *非* 随内容一起发布。 | CUG策略 *是* 随内容一起发布。 |

## 支持链接 {#supporting-links}

* [管理资产和封闭用户组](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [创建已关闭的用户组](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Oak封闭用户组文档](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
