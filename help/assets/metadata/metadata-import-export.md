---
title: 在AEM Assets中使用元数据导入和导出
description: 了解如何使用Adobe Experience Manager Assets的导入和导出元数据功能。 导入和导出功能允许内容作者批量更新现有资产的元数据。
version: 6.4, 6.5, Cloud Service
topic: Content Management
feature: Metadata
role: Admin
level: Intermediate
kt: 647, 917
thumbnail: 22132.jpg
last-substantial-update: 2022-06-13T00:00:00Z
exl-id: 0681e2c4-8661-436c-9170-9aa841a6fa27
source-git-commit: f37483f90f2a707c906e1e206795fdebb5f698e9
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 3%

---

# 在AEM Assets中使用元数据导入和导出 {#metadata-import-and-export}

了解如何使用Adobe Experience Manager Assets的导入和导出元数据功能。 导入和导出功能允许内容作者批量更新现有资产的元数据。

## 元数据导出 {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132/?quality=12&learn=on)

## 元数据导入 {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374/?quality=12&learn=on)

>[!NOTE]
>
> 在准备导入CSV文件时，使用元数据导出功能更容易生成包含资产列表的CSV。 然后，您可以修改生成的CSV文件，并使用“导入”功能导入它。

## 元数据CSV文件格式 {#metadata-file-format}

### 第一行

* CSV文件的第一行定义了元数据架构。
* 第一列默认为 `assetPath`，其中包含资产的绝对JCR路径。

* 第一行中的后续列指向资产的其他元数据属性。
   * 例如：`dc:title, dc:description, jcr:title`

* 单值属性格式

   * `<metadata property name> {{<property type}}`
   * 如果未指定属性类型，则其默认为String。
   * 例如：`dc:title {{String}}`

* 属性名称区分大小写
   * 正确： `dc:title {{String}}`
   * 错误： `Dc:Title {{String}}`

* 属性类型不区分大小写
* 所有有效 [JCR属性类型](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) 支持

* 多值属性格式 —  `<metadata property name> {{<property type : MULTI }}`

### 第二行到N行

* 第一列保存资产的绝对JCR路径。 例如：/content/dam/asset1.jpg
* 资产的元数据属性在CSV文件中可能缺少值。 该特定资产缺少的元数据属性不会更新。
