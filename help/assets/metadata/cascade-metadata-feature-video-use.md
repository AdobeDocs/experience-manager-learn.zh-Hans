---
title: 在AEM Assets使用级联元数据
seo-title: 在AEM Assets使用级联元数据
description: 高级元数据管理允许用户创建级联字段规则，在AEM Assets的元数据之间形成上下文关系。 以下视频演示了有关字段要求、可见性和上下文选择的新动态规则。 该视频还详细介绍了管理员将这些规则应用到自定义元数据模式所需的步骤。
seo-description: 高级元数据管理允许用户创建级联字段规则，在AEM Assets的元数据之间形成上下文关系。 以下视频演示了有关字段要求、可见性和上下文选择的新动态规则。 该视频还详细介绍了管理员将这些规则应用到自定义元数据模式所需的步骤。
uuid: 470c1b1a-f888-4c90-87d7-acfa9a5fa6b1
discoiquuid: ccd1acb1-bb7f-48c2-91e0-cccbeedad831
topics: metadata
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---


# 在AEM Assets使用级联元数据{#using-cascading-metadata-in-aem-assets}

高级元数据管理允许用户创建级联字段规则，在AEM Assets的元数据之间形成上下文关系。 以下视频演示了有关字段要求、可见性和上下文选择的新动态规则。 该视频还详细介绍了管理员将这些规则应用到自定义元数据模式所需的步骤。

>[!VIDEO](https://video.tv.adobe.com/v/20702/?quality=9&learn=on)

可以为给定元数据字段启用三个动态规则集：

1. **要求** :可以根据另一个下拉字段的值动态标记为“必需”。

2. **可见性** :字段始终可见或仅可见，这取决于其他下拉字段的值。

3. **选择** :（仅适用于下拉字段）根据当前选择的其他下拉字段值筛选显示给用户的选项。

>[!NOTE]
>
>只能根据下拉字段的值创建级联规则。 可以将所有三个规则集应用到同一元数据字段，但作为最佳实践，建议将每个规则集都依赖于同一元数据下拉列表。

下载自 [定义元数据包](assets/cascade-metadata-values-001.zip)

## 其他资源{#additional-resources}

自定义元数据模式创建于： `/conf/global/settings/dam/adminui-extension/metadataschema/custom`. 以下AEM包将将自定义模式应用于文件夹： `/content/dam/we-retail/en/activities`:

