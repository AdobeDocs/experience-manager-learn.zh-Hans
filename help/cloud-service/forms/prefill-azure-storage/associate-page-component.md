---
title: 将页面组件与新的自适应表单模板关联
description: 创建新页面组件
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 5%

---

# 将页面组件与模板关联

下一步是将页面组件与新的自适应表单模板关联。 这可确保每次基于新模板的自适应表单呈现时，都会执行页面组件中的代码。 在本教程中，新增了一个自适应表单模板，名为 **StoreAndRestoreFromAzure** 创建于 **AzurePortalStorage** 文件夹。
导航到/conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/initial/jcr：content节点，添加以下属性并保存更改。

| **属性名称** | **属性类型** | **属性值** |
|--------------------|-------------------|-------------------------------------------------------|
| sling:resourceType | 字符串 | azureportalpagecomponent/component/page/storeandfetch |

导航到/conf/AzurePortalStorage/settings/wcm/templates/storeandrestorefromazure/structure/jcr：content节点，添加以下属性并保存更改。
| **属性名称**  | **属性类型** | **属性值**                                    | --------------------结-------------------结------------------------------------------------------- | sling：resourceType |字符串 | azureportalpagecomponent/component/page/storeandfetch |


## 后续步骤

[创建与Azure存储的集成](./create-fdm.md)
