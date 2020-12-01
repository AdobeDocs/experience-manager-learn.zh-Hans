---
title: AEM Assets的封闭用户组
description: '“已关闭的用户组”(CUG)是一项功能，用于限制对已发布站点上的选定用户组的内容访问权限。 此视频显示如何将已关闭的用户组与Adobe Experience Manager资产结合使用，以限制对特定资产文件夹的访问。 首先在AEM 6.4中引入了对AEM Assets封闭用户组的支持。 '
feature: asset-share
topics: authoring, collaboration, operations, sharing
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 10784dce34443adfa1fc6dc324242b1c021d2a17
workflow-type: tm+mt
source-wordcount: '486'
ht-degree: 0%

---


# 已关闭的用户组{#using-closed-user-groups-with-aem-assets}

“已关闭的用户组”(CUG)是一项功能，用于限制对已发布站点上的选定用户组的内容访问权限。 此视频显示如何将已关闭的用户组与Adobe Experience Manager资产结合使用，以限制对特定资产文件夹的访问。 首先在AEM 6.4中引入了对AEM Assets封闭用户组的支持。

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=9&learn=on)

## Closed User Group(CUG)withAEM Assets

* 旨在限制对AEM发布实例上的资产的访问。
* 授予对一组用户／组的读取权限。
* CUG只能在文件夹级别进行配置。 无法对单个资产设置CUG。
* CUG策略由任何子文件夹和已应用的资产自动继承。
* 通过设置新的CUG策略，CUG策略可以被子文件夹覆盖。 这应该少用一些，不应被视为最佳做法。

## JCR {#cug-representation-in-the-jcr}中的CUG表示

![JCR中的CUG表示](assets/closed-user-groups/folder-properties-closed-user-groups.png)

We.Retail成员组添加为已关闭的用户组到文件夹：/content/dam/we-retail/en/beta-products

将&#x200B;**rep:CugMixin**&#x200B;的mixin应用于&#x200B;**/content/dam/we-retail/en/beta-products**&#x200B;文件夹。 在文件夹下添加&#x200B;**rep:cugPolicy**&#x200B;的节点，并将we-retail-members指定为主体。 **granite:AuthenticationRequired**&#x200B;的另一个混音将应用于beta-products文件夹，属性** granite:loginPath**指定用户未通过身份验证并尝试请求位于&#x200B;**beta-products**&#x200B;文件夹下的资产时要使用的登录页。

JCR说明如下：

```xml
/beta-products
    - jcr:primaryType = sling:Folder
    - jcr:mixinTypes = rep:CugMixin, granite:AuthenticationRequired
    - granite:loginPath = /content/we-retail/us/en/community/signin
    + rep:cugPolicy
         - jcr:primaryType = rep:CugPolicy
         - rep:principalNames = we-retail-members
```

## 已关闭的用户组与访问控制列表{#closed-user-groups-vs-access-control-lists}

已关闭的用户组(CUG)和访问控制列表(ACL)都用于控制对AEM中的内容以及基于AEM安全用户和组的内容的访问。 但是这些功能的应用和实现却大不相同。 下表总结了两个特征之间的区别。

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| 预期用途 | 在&#x200B;**current** AEM实例上配置和应用内容的权限。 | 为AEM **author**&#x200B;实例上的内容配置CUG策略。 对AEM **publish**&#x200B;实例上的内容应用CUG策略。 |
| 权限级别 | 定义所有级别的用户／组的已授予／已拒绝权限：读取、修改、创建、删除、读取ACL、编辑ACL、复制。 | 授予对一组用户／组的读取权限。 拒绝对所有其他用户／组的读取访问。 |
| 复制 | ACL不与内容复制。 | CUG策略与内容一起复制。 |

## 支持链接{#supporting-links}

* [管理资产和已关闭的用户组](https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html#ClosedUserGroup)
* [创建已关闭的用户组](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/cug.html)
* [Oak已关闭的用户组文档](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
