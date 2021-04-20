---
title: 在AEM Forms打印渠道文档中使用表组件
seo-title: 在AEM Forms打印渠道文档中使用表组件
description: 以下视频将逐步介绍在Interactive Communications中使用表组件进行打印渠道文档所需的步骤。
feature: Interactive Communication
topics: development
audience: developer
doc-type: technical video
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 2%

---


# 在AEM Forms打印渠道文档{#using-table-component-in-aem-forms-print-channel-document}中使用表组件

以下视频将逐步介绍在Interactive Communications中使用表组件进行打印渠道文档所需的步骤。

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=9&learn=on)

表格用于以表格形式显示数据。 表中的行需要根据数据源返回的数据增大或缩小。 要在打印渠道文档中使用表，我们需要使用AEM Forms Designer创建布局文件（xdp文件）。 在此布局文件中，我们添加具有所需列数的表。 根据您的要求，确保列字段对象类型为TextField或Numeric Field。 对于每列，字段确保数据绑定设置为使用名称。

>[!NOTE]
>
>要使表动态化，请确保已将行标记为重复。

**在您自己的服务器上试用它**

* [下载资产文件并将其解压缩到您的硬盘上](assets/usingtablesinprintchannel.zip)

* 使用包管理器将两个zip文件导入AEM

* 与本文关联的资产包括：

   * 布局片段

   * 表单数据模式

   * 交互式通信文档
   * sampleretirementaccountdata.json

* 在[编辑模式](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html)中打开交互通信文档。

* 将TableDemo布局片段添加到贡献部分。
* 将表单元格绑定到视频中所示的相应表单数据模型元素

* 预览 Interactive Communication文档与提供给您的示例json数据文件

