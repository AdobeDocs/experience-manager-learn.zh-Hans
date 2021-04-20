---
title: 在AEM Assets中使用元数据导入和导出
description: 了解如何使用Adobe Experience Manager Assets的导入和导出元数据功能。 导入和导出功能允许内容作者批量更新现有资产的元数据。
version: 6.3, 6.4, 6.5, cloud-service
topic: Content Management
feature: Metadata
role: Admin
level: Intermediate
kt: 647, 917
thumbnail: 22132.jpg
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 4%

---


# 在AEM Assets {#metadata-import-and-export}中使用元数据导入和导出

了解如何使用Adobe Experience Manager Assets的导入和导出元数据功能。 导入和导出功能允许内容作者批量更新现有资产的元数据。

## 元数据导出 {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132/?quality=12&learn=on)

## 元数据导入 {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374/?quality=12&learn=on)

>[!NOTE]
>
> 在准备要导入的CSV文件时，使用元数据导出功能可以更轻松地生成具有资产列表的CSV。 然后，您可以修改生成的CSV文件，并使用“导入”功能导入它。

## 元数据CSV文件格式{#metadata-file-format}

### 第一行

* CSV文件的第一行定义元数据模式。
* “第一列”默认为`assetPath`，其中包含资产的绝对JCR路径。

* 第一行中的后续列指向资产的其他元数据属性。
   * 例如：`dc:title, dc:description, jcr:title`

* 单值属性格式

   * `<metadata property name> {{<property type}}`
   * 如果未指定属性类型，则默认为String。
   * 例如：`dc:title {{String}}`

* 属性名称区分大小写
   * 正确：`dc:title {{String}}`
   * 不正确：`Dc:Title {{String}}`

* 属性类型不区分大小写
* 支持所有有效的[JCR属性类型](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html)

* 多值属性格式 — `<metadata property name> {{<property type : MULTI }}`

### 第二行到N行

* 第一列保存资产的绝对JCR路径。 例如：/content/dam/asset1.jpg
* 资产的元数据属性可能在CSV文件中缺少值。 该特定资产缺少的元数据属性不会更新。
