---
title: 在AEM Forms打印渠道文档中使用表组件
seo-title: 在AEM Forms打印渠道文档中使用表组件
description: 以下视频将逐步介绍在Interactive Communications中使用表组件进行打印渠道文档所需的步骤。
feature: interactive-communication
topics: development
audience: developer
doc-type: technical video
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 0%

---


# 在AEM Forms打印渠道文档中使用表组件 {#using-table-component-in-aem-forms-print-channel-document}

以下视频将逐步介绍在Interactive Communications中使用表组件进行打印渠道文档所需的步骤。

>[!VIDEO](https://video.tv.adobe.com/v/27769?quality=9&learn=on)

表用于以表格形式显示数据。 表中的行需要根据数据源返回的数据增大或缩小。 要在打印渠道文档中使用表，我们需要使用AEM Forms设计器创建布局文件（xdp文件）。 在此布局文件中，我们添加具有所需列数的表。 根据您的要求，确保列字段对象类型为TextField或数字字段。 对于每列，字段确保数据绑定设置为“使用名称”。

>[!NOTE]
要使表动态化，请确保已将“行”标记为重复。

**在您自己的服务器上试用它**

* [下载资产文件并将其解压缩到硬盘上](assets/usingtablesinprintchannel.zip)

* 使用包管理器将两个zip文件导入AEM

* 与本文关联的资产包括：

   * 布局片段

   * 表单数据模式

   * 交互通信文档
   * sampleretirementaccountdata.json

* 在编辑模式下打开交互式 [通信文档](http://localhost:4502/editor.html/content/forms/af/401kstatement/tablesinprintdocument/channels/print.html)。

* 将TableDemo布局片段添加到贡献部分。
* 将表单元格绑定到相应的表单数据模型元素，如视频所示

* 预览交互式通信文档，其中包含提供给您的示例json数据文件

