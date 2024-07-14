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
source-git-commit: 726715890d997ba3bb85f4833e220ac2222b3a42
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 1%

---

# 在AEM Assets中使用元数据导入和导出 {#metadata-import-and-export}

了解如何使用Adobe Experience Manager Assets的导入和导出元数据功能。 导入和导出功能允许内容作者批量更新现有资源的元数据。

## 元数据导出 {#metadata-export}

>[!VIDEO](https://video.tv.adobe.com/v/22132?quality=12&learn=on)

>[!TIP]
>
> 在Excel中打开元数据导出CSV文件时，请使用[Excel导入程序](https://support.microsoft.com/en-us/office/import-data-from-a-csv-html-or-text-file-b62efe49-4d5b-4429-b788-e1211b5e90f6)，而不是双击该文件，以避免UTF-8编码的CSV文件出现问题。
>
> 要在Excel中打开元数据导出CSV文件，请执行以下步骤：
> 
> 1. 打开Microsoft Excel
> 1. 选择&#x200B;__文件>新建__&#x200B;以创建空电子表格
> 1. 打开空电子表格，选择&#x200B;__文件>导入__
> 1. 选择&#x200B;__文本__&#x200B;文件并单击&#x200B;__导入__
> 1. 从文件系统中选择导出的CSV文件，然后单击&#x200B;__获取数据__
> 1. 在导入向导的第1步中，选择&#x200B;__分隔__&#x200B;并将&#x200B;__文件源__&#x200B;设置为&#x200B;__Unicode (UTF-8)__，然后单击&#x200B;__下一步__
> 1. 在步骤2中，将&#x200B;__分隔符__&#x200B;设置为&#x200B;__逗号__，然后单击&#x200B;__下一步__
> 1. 在步骤3中，将&#x200B;__列数据格式__&#x200B;保持原样，然后单击&#x200B;__完成__
> 1. 选择&#x200B;__导入__&#x200B;将数据添加到电子表格

## 元数据导入 {#metadata-import}

>[!VIDEO](https://video.tv.adobe.com/v/21374?quality=12&learn=on)

>[!NOTE]
>
> 在准备导入的CSV文件时，使用“元数据导出”功能可更轻松地生成包含资源列表的CSV。 然后，您可以修改生成的CSV文件，并使用“导入”功能导入该文件。

## 元数据CSV文件格式 {#metadata-file-format}

### 第一行

* CSV文件的第一行定义元数据架构。
* 第一列默认为`assetPath`，它保存资产的绝对JCR路径。

* 第一行中的后续列指向资源的其他元数据属性。
   * 例如： `dc:title, dc:description, jcr:title`

* 单值属性格式

   * `<metadata property name> {{<property type}}`
   * 如果未指定属性类型，则默认为String。
   * 例如：`dc:title {{String}}`

* 属性名称区分大小写
   * 正确：`dc:title {{String}}`
   * 不正确： `Dc:Title {{String}}`

* 属性类型区分大小写
* 支持所有有效的[JCR属性类型](https://www.adobe.io/experience-manager/reference-materials/spec/jsr170/javadocs/jcr-2.0/javax/jcr/PropertyType.html)

* 多值属性格式 — `<metadata property name> {{<property type : MULTI }}`

### 第二行到N行

* 第一列包含资源的绝对JCR路径。 例如： /content/dam/asset1.jpg
* 资源的元数据属性在CSV文件中可能缺少值。 缺少该特定资源的元数据属性不会更新。
