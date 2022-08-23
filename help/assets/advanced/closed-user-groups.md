---
title: AEM Assets中的已关闭用户组
description: “已关闭的用户组”(CUG)是一项功能，用于限制对已发布网站上的选定用户组的内容访问权限。 此视频演示了如何将已关闭的用户组与Adobe Experience Manager Assets一起使用，以限制对特定资产文件夹的访问。
version: 6.4, 6.5, Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
kt: 649
thumbnail: 22155.jpg
exl-id: a2bf8a82-15ee-478c-b7c3-de8a991dfeb8
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 0%

---

# 已关闭的用户组{#using-closed-user-groups-with-aem-assets}

“已关闭的用户组”(CUG)是一项功能，用于限制对已发布网站上的选定用户组的内容访问权限。 此视频演示了如何将已关闭的用户组与Adobe Experience Manager Assets一起使用，以限制对特定资产文件夹的访问。 AEM 6.4中首次引入了对包含AEM Assets的封闭用户组的支持。

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## 封闭用户组(CUG)(含AEM Assets)

* 旨在限制对AEM发布实例上资产的访问。
* 授予对一组用户/组的读取权限。
* CUG只能在文件夹级别配置。 无法对单个资产设置CUG。
* 任何子文件夹和已应用的资产会自动继承CUG策略。
* 通过设置新的CUG策略，CUG策略可被子文件夹覆盖。 应谨慎使用此方法，而不应视为最佳做法。

## 已关闭的用户组与访问控制列表 {#closed-user-groups-vs-access-control-lists}

已关闭的用户组(CUG)和访问控制列表(ACL)用于根据AEM Security用户和组控制对AEM中内容的访问。 但是这些功能的应用和实现却截然不同。 下表总结了两个特征之间的区别。

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| 预期用途 | 在 **当前** AEM实例。 | 在AEM上为内容配置CUG策略 **作者** 实例。 在AEM上对内容应用CUG策略 **发布** 实例。 |
| 权限级别 | 为所有级别的用户/组定义已授予/已拒绝的权限：读取、修改、创建、删除、读取ACL、编辑ACL、复制。 | 授予对一组用户/组的读取权限。 拒绝对的读取访问 *所有其他* 用户/群组。 |
| 发布 | ACL为 *not* 已发布内容。 | CUG策略 *are* 已发布内容。 |

## 支持链接 {#supporting-links}

* [管理资产和已关闭的用户组](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [创建封闭用户组](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Oak封闭用户组文档](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
