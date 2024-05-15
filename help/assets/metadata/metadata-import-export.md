---
title: 在AEM Assets中使用元数据导入和导出
description: 了解如何使用Adobe Experience Manager Assets的导入和导出元数据功能。 导入和导出功能允许内容作者批量更新现有资源的元数据。
version: 6.4, 6.5, Cloud Service
topic: Content Management
feature: Metadata
role: Admin
level: Intermediate
kt: 647, 917
thumbnail: 22132.jpg
last-substantial-update: 2022-06-13T00:00:00Z
doc-type: Feature Video
exl-id: 0681e2c4-8661-436c-9170-9aa841a6fa27
duration: 419
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 2%

---

# 在AEM Assets中使用元数据导入和导出 {#metadata-import-and-export}

了解如何使用Adobe Experience Manager Assets的导入和导出元数据功能。 导入和导出功能允许内容作者批量更新现有资源的元数据。

## 元数据导出 {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132?quality=12&learn=on)

## 元数据导入 {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374?quality=12&learn=on)

>[!NOTE]
>
> 在准备导入的CSV文件时，使用“元数据导出”功能可更轻松地生成包含资源列表的CSV。 然后，您可以修改生成的CSV文件，并使用“导入”功能导入该文件。

## 元数据CSV文件格式 {#metadata-file-format}

### 第一行

* CSV文件的第一行定义元数据架构。
* 第一列默认为 `assetPath`，保存资产的绝对JCR路径。

* 第一行中的后续列指向资源的其他元数据属性。
   * 例如： `dc:title, dc:description, jcr:title`

* 单值属性格式

   * `<metadata property name> {{<property type}}`
   * 如果未指定属性类型，则默认为String。
   * 例如：`dc:title {{String}}`

* 属性名称区分大小写
   * 正确： `dc:title {{String}}`
   * 不正确： `Dc:Title {{String}}`

* 属性类型区分大小写
* 全部有效 [JCR属性类型](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) 受支持

* 多值属性格式 —  `<metadata property name> {{<property type : MULTI }}`

### 第二行到N行

* 第一列包含资源的绝对JCR路径。 例如： /content/dam/asset1.jpg
* 资源的元数据属性在CSV文件中可能缺少值。 缺少该特定资源的元数据属性不会更新。
