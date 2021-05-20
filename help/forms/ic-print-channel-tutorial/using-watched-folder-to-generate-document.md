---
title: 使用监视文件夹生成打印渠道文档
seo-title: 使用监视文件夹生成打印渠道文档
description: 这是为打印渠道创建首个交互式通信文档的多步教程的10部分。 在本部分中，我们将使用监视文件夹机制生成打印渠道文档。
seo-description: 这是为打印渠道创建首个交互式通信文档的多步教程的10部分。 在本部分中，我们将使用监视文件夹机制生成打印渠道文档。
uuid: 9e39f4e3-1053-4839-9338-09961ac54f81
feature: 交互式通信
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
contentOwner: gbedekar
discoiquuid: 23fbada3-d776-4b77-b381-22d3ec716ae9
topic: 开发
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 0%

---


# 使用监视文件夹生成打印渠道文档

在本部分中，我们将使用监视文件夹机制生成打印渠道文档。

在创建和测试打印渠道文档后，我们需要一种机制以批量模式或按需生成这些文档。 通常，这些类型的文档会以批量模式生成，最常见的机制是使用监视文件夹。

在AEM中配置监视文件夹时，会关联在将文件放入监视文件夹后执行的ECMA脚本或Java代码。 在本文中，我们将重点介绍ECMA脚本，该脚本将生成打印渠道文档并将其保存到文件系统中。

已监视的文件夹配置和ECMA脚本是您在本教程[开头导入的资产的一部分](introduction.md)

放入监视文件夹的输入文件具有以下结构。 ECMA脚本读取帐号，并为每个帐户生成打印渠道文档。

有关用于生成文档的ECMA脚本的更多详细信息，请[参阅本文](/help/forms/interactive-communications/generating-interactive-communications-print-document-using-api-tutorial-use.md)

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

要使用监视文件夹机制生成打印渠道文档，请执行以下步骤：

* [按照本文档中所述的步骤操作](/help/forms/adaptive-forms/service-user-tutorial-develop.md)

* 登录到crx并导航到/etc/fd/watchfolder/scripts/PrintPDF.ecma

* 确保interactiveCommunicationsDocument的路径指向要打印的正确文档。（第1行）
* 记下saveLocation（第2行）。您可以根据需要更改它。
* 确保表单数据模型的输入参数绑定到请求属性，并且其绑定值设置为“accountnumber”。 请参阅下面的屏幕截图。
   ![请求](assets/requestattributeprintchannel.gif)

* 创建包含以下内容的accountnumbers.xml文件

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

* 将xml文件放入C:\RenderPrintChannel\input中

* 按照ECMA脚本中的指定，在保存位置中检查pdf文件。




