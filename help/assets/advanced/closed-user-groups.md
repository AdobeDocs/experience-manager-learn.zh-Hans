---
title: AEM Assets中的封闭用户组
description: 封闭用户组(CUG)是一项功能，它仅限一组精选的用户访问已发布的网站上的内容。 此视频展示封闭用户组如何与Adobe Experience Manager Assets配合使用以限制对资源的特定文件夹的访问。
version: 6.4, 6.5, Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-649
thumbnail: 22155.jpg
last-substantial-update: 2022-06-06T00:00:00Z
doc-type: Feature Video
exl-id: a2bf8a82-15ee-478c-b7c3-de8a991dfeb8
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 0%

---

# 已关闭的用户组{#using-closed-user-groups-with-aem-assets}

封闭用户组(CUG)是一项功能，它仅限一组精选的用户访问已发布的网站上的内容。 此视频展示封闭用户组如何与Adobe Experience Manager Assets配合使用以限制对资源的特定文件夹的访问。 AEM 6.4中首次引入了AEM Assets对封闭用户组的支持。

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## 包含AEM Assets的封闭用户组(CUG)

* 旨在限制对AEM Publish实例上资源的访问。
* 授予对一组用户/组的读取权限。
* CUG只能在文件夹级别进行配置。 无法在单个资产上设置CUG。
* 任何子文件夹和应用的资产都会自动继承CUG策略。
* 通过设置新的CUG策略，子文件夹可以覆盖CUG策略。 应谨慎使用此方法，不应将其视为最佳实践。

## 已关闭的用户组与访问控制列表 {#closed-user-groups-vs-access-control-lists}

封闭用户组(CUG)和访问控制列表(ACL)都可以根据AEM Security用户和组来控制对AEM中内容的访问。 然而，这些功能的应用和实现却大不相同。 下表总结了这两项功能之间的区别。

|                   | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| 预期用途 | 为上的内容配置和应用权限 **当前** AEM实例。 | 为AEM上的内容配置CUG策略 **作者** 实例。 对AEM上的内容应用CUG策略 **发布** 实例。 |
| 权限级别 | 为用户/组定义所有级别的授予/拒绝权限：读取、修改、创建、删除、读取ACL、编辑ACL、复制。 | 授予对一组用户/组的读取权限。 拒绝对的读取权限 *所有其他* 用户/组。 |
| 发布 | ACL是 *非* 已发布内容。 | CUG策略 *是* 已发布内容。 |

## 支持链接 {#supporting-links}

* [管理资产和封闭用户组](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [创建已关闭的用户组](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Oak封闭用户组文档](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
