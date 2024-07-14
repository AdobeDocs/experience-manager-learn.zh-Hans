---
title: 将页面组件与新的自适应表单模板关联
description: 创建新页面组件
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
exl-id: 7b2b1e1c-820f-4387-a78b-5d889c31eec0
duration: 25
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 4%

---

# 将页面组件与模板关联

下一步是将页面组件与新的自适应表单模板关联。 这可确保在每次基于新模板的自适应表单呈现时都执行页面组件中的代码。 出于本教程的目的，在&#x200B;**AzurePortalStorage**&#x200B;文件夹中创建了一个名为&#x200B;**StoreAndRestoreFromAzure**的新自适应表单模板。
导航到/conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/initial/jcr：content节点，添加以下属性并保存更改。

| **属性名称** | **属性类型** | **属性值** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | 字符串 | azureportalpagecomponent/component/page/storeandfetch |

导航到/conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/structure/jcr：content节点，添加以下属性并保存更改。
| **属性名称**  | **属性类型** | **属性值**                                    |
---------------------------------------文-------------------------------------------------------
| sling：resourceType | 字符串            | azureportalpagecomponent/component/page/storeandfetch |


## 后续步骤

[创建与Azure存储的集成](./create-fdm.md)
