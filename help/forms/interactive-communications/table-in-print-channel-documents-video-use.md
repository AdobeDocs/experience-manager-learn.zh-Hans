---
title: 在AEM Forms打印渠道文档中使用表组件
seo-title: Using Table Component in AEM Forms Print Channel Document
description: 以下视频将演示在交互式通信中使用表组件打印渠道文档所需的步骤。
feature: Interactive Communication
topics: development
audience: developer
doc-type: technical video
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 54afd047-c6e6-4557-9336-39420f30df88
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '267'
ht-degree: 0%

---

# 在AEM Forms打印渠道文档中使用表组件 {#using-table-component-in-aem-forms-print-channel-document}

以下视频将演示在交互式通信中使用表组件打印渠道文档所需的步骤。

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=9&learn=on)

表格用于以表格形式显示数据。 表中的行需要根据数据源返回的数据而增大或收缩。 要在打印渠道文档中使用表，我们需要使用AEM Forms Designer创建布局文件（xdp文件）。 在此布局文件中，我们添加了具有所需列数的表。 根据您的要求，确保列字段对象类型为TextField或Numeric Field 。 对于每列，字段会确保数据绑定设置为“使用名称”。

>[!NOTE]
>
>要使表动态，请确保将行标记为重复。

**在您自己的服务器上尝试**

* [将资产文件下载并解压缩到硬盘上](assets/usingtablesinprintchannel.zip)

* 使用包管理器将两个zip文件导入AEM

* 与本文关联的资产中包含以下内容：

   * 布局片段

   * 表单数据模式

   * 交互式通信文档
   * sampleretirementaccountdata.json

* 在中打开交互式通信文档 [编辑模式](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html).

* 将TableDemo布局片段添加到贡献部分。
* 将表单元格绑定到相应的表单数据模型元素，如视频中所示

* 预览交互式通信文档，其中提供给您的示例json数据文件
