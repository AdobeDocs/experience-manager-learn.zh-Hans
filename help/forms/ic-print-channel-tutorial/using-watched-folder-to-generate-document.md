---
title: 使用监视的文件夹生成打印渠道文档
seo-title: 使用监视的文件夹生成打印渠道文档
description: 这是创建打印文档的第一个交互式通信渠道的多步教程的第10部分。 在此部分，我们将使用监视的文件夹机制生成打印渠道文档。
seo-description: 这是创建打印文档的第一个交互式通信渠道的多步教程的第10部分。 在此部分，我们将使用监视的文件夹机制生成打印渠道文档。
uuid: 9e39f4e3-1053-4839-9338-09961ac54f81
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
contentOwner: gbedekar
discoiquuid: 23fbada3-d776-4b77-b381-22d3ec716ae9
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 0%

---


# 使用监视的文件夹生成打印渠道文档

在此部分，我们将使用监视的文件夹机制生成打印渠道文档。

创建和测试打印渠道文档后，我们需要一种机制以批处理模式或按需生成这些文档。 通常，这些类型的文档以批处理模式生成，最常见的机制是使用监视的文件夹。

在AEM中配置监视文件夹时，将关联在将文件放入监视文件夹时执行的ECMA脚本或java代码。 本文将重点介绍ECMA脚本，它将生成打印渠道文档并将其保存到文件系统。

监视的文件夹配置和ECMA脚本是您在本教程](introduction.md)开头导入的[资源的一部分

放入监视文件夹的输入文件具有以下结构。 ECMA脚本读取帐户号并为每个帐户生成打印渠道文档。

有关生成文档的ECMA脚本的详细信息，[请参阅本文](/help/forms/interactive-communications/generating-interactive-communications-print-document-using-api-tutorial-use.md)

```xml
<accountnumbers>
 <accountnumber>509840</accountnumber>
 <accountnumber>948576</accountnumber>
 <accountnumber>398762</accountnumber>
 <accountnumber>291723</accountnumber>
 <accountnumber>291724</accountnumber>
 <accountnumber>291725</accountnumber>
 <accountnumber>291726</accountnumber>
 <accountnumber>291727</accountnumber>
</accountnumbers>
```

要使用监视的文件夹机制生成打印渠道文档，请执行以下步骤：

* [按照本文档中所述的步骤操作](/help/forms/adaptive-forms/service-user-tutorial-develop.md)

* 登录crx并导航到/etc/fd/watchfolder/scripts/PrintPDF.ecma

* 确保interactiveCommunicationsDocument的路径指向要打印的正确文档。（第1行）
* 记下saveLocation（第2行）。您可以根据需要更改它。
* 确保表单数据模型的输入参数绑定到请求属性，且其绑定值设置为“accountnumber”。 请参阅下面的屏幕截图。
   ![请求](assets/requestattributeprintchannel.gif)

* 使用以下内容创建accountnumbers.xml文件

```xml
<accountnumbers>
<accountnumber>1</accountnumber>
<accountnumber>100</accountnumber>
<accountnumber>101</accountnumber>
<accountnumber>1009</accountnumber>
<accountnumber>10009</accountnumber>
<accountnumber>11990</accountnumber>
</accountnumbers>
```

* 将xml文件放入C:\RenderPrintChannel\input

* 按照ECMA脚本中的指定检查保存位置中的pdf文件。




