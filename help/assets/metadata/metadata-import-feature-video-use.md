---
title: 在AEM Assets使用元数据导入和导出
seo-title: 在AEM Assets使用元数据导入和导出
description: AEM Assets元数据导入和导出功能使内容作者能够轻松地在AEM中移入和移出资产元数据，并利用Microsoft Excel的强大功能大规模处理元数据，从而便于AEM中现有资产的批量更新元数据。
seo-description: AEM Assets元数据导入和导出功能使内容作者能够轻松地在AEM中移入和移出资产元数据，并利用Microsoft Excel的强大功能大规模处理元数据，从而便于AEM中现有资产的批量更新元数据。
uuid: db7e57a4-b0c1-4a48-906d-802c19964313
discoiquuid: 72dd9230-73e1-454e-a3e0-9281e621d901
topics: metadata
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 2%

---


# 在AEM Assets使用元数据导入和导出{#using-metadata-import-and-export-in-aem-assets}

AEM Assets元数据导入和导出功能使内容作者能够轻松地在AEM中移入和移出资产元数据，并利用Microsoft Excel的强大功能大规模处理元数据，从而便于AEM中现有资产的批量更新元数据。

## 元数据导出 {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132/?quality=9&learn=on)

## 元数据导入 {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374/?quality=9&learn=on)

下载 [WeRetail sports文件夹](assets/we-retail-sports.zip)

下载 [资产元数据包](assets/we-retail-sports-asset-metadata.zip)

## 元数据文件格式 {#metadata-file-format}

### CSV文件格式

#### 第一行

* CSV文件的第一行定义元数据模式。
* “第一列”默 `assetPath`认为，它包含资产的绝对JCR路径。

* 第一行中的后续列指向资产的其他元数据属性。

   * 例如：`dc:title, dc:description, jcr:title`

* 单值属性格式

   * `<metadata property name> {{<property type}}`
   * 如果未指定属性类型，则默认为字符串。
   * 例如：`dc:title {{String}}`

* 属性名称区分大小写
   * 正确： `dc:title {{String}}`
   * 不正确： `Dc:Ttle {{String}}`

* 属性类型不区分大小写
* 支持所 [有有效的JCR属性](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html) 类型

* 多值属性格式- `<metadata property name> {{<property type : MULTI }}`

#### 第二行到N行

* 第一列保存资产的绝对JCR路径。 例如：/content/dam/asset1.jpg
* 资产的元数据属性可能在CSV文件中缺少值。 该特定资产缺少的元数据属性将不会更新。