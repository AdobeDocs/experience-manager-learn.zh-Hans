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
duration: 45
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 4%

---

# 将页面组件与模板关联

下一步是将页面组件与新的自适应表单模板关联。 这可确保在每次基于新模板的自适应表单呈现时都执行页面组件中的代码。 在本教程中，新增了一个自适应表单模板，名为 **StoreAndRestoreFromAzure** 创建于 **AzurePortalStorage** 文件夹。
导航到/conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/initial/jcr：content节点，添加以下属性并保存更改。

| **属性名称** | **属性类型** | **属性值** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | 字符串 | azureportalpagecomponent/component/page/storeandfetch |

导航到/conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/structure/jcr：content节点，添加以下属性并保存更改。
| **属性名称**  | **属性类型** | **属性值**                                    | ---------------------------------------文------------------------------------------------------- | sling：resourceType |字符串 | azureportalpagecomponent/component/page/storeandfetch |


## 后续步骤

[创建与Azure存储的集成](./create-fdm.md)
