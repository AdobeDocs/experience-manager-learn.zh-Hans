---
title: 在AEM Forms Print Channel文档中使用表组件
seo-title: Using Table Component in AEM Forms Print Channel Document
description: 以下视频介绍了在交互式通信中将表组件用于打印渠道文档所需的步骤。
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
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '267'
ht-degree: 0%

---

# 在AEM Forms Print Channel文档中使用表组件 {#using-table-component-in-aem-forms-print-channel-document}

以下视频介绍了在交互式通信中将表组件用于打印渠道文档所需的步骤。

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=12&learn=on)

表格用于以表格方式显示数据。 表中的行需要根据数据源返回的数据进行增大或缩小。 要在打印渠道文档中使用表，需要使用AEM Forms Designer创建布局文件（xdp文件）。 在此布局文件中，我们添加具有所需列数的表。 确保列字段对象类型是TextField或Numeric Field，具体取决于您的要求。 对于每一列，字段确保将数据绑定设置为“使用名称”。

>[!NOTE]
>
>要使表格成为动态表格，请确保已将该行标记为重复。

**在您自己的服务器上尝试**

* [将资源文件下载并解压缩到硬盘上](assets/usingtablesinprintchannel.zip)

* 使用包管理器将两个zip文件导入AEM

* 与本文关联的资产包括以下内容：

   * 布局片段

   * 表单数据模式

   * 交互式通信文档
   * sampleretirementaccountdata.json

* 在中打开交互式通信文档 [编辑模式](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html).

* 将TableDemo布局片段添加到贡献部分。
* 将表单元格绑定到适当的表单数据模型元素，如视频中所示

* 使用提供给您的示例json数据文件预览交互式通信文档
