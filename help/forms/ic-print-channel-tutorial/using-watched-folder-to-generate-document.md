---
title: 使用Watched文件夹生成打印渠道文档
seo-title: Generating Print Channel Documents Using Watched Folder
description: 这是为打印渠道创建第一个交互式通信文档的多步教程的第10部分。 在本部分中，我们将使用watched文件夹机制生成打印渠道文档。
seo-description: This is part 10 of multistep tutorial for creating your first interactive communications document for the print channel. In this part, we will generate print channel documents using the watched folder mechanism.
uuid: 9e39f4e3-1053-4839-9338-09961ac54f81
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
contentOwner: gbedekar
discoiquuid: 23fbada3-d776-4b77-b381-22d3ec716ae9
topic: Development
role: Developer
level: Beginner
exl-id: 9bb05c94-2a7b-4149-b567-186eb08b1c66
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '348'
ht-degree: 0%

---

# 使用Watched文件夹生成打印渠道文档

在本部分中，我们将使用watched文件夹机制生成打印渠道文档。

创建并测试打印渠道文档后，我们需要一种机制以批量模式或按需生成这些文档。 通常，此类文档以批处理模式生成，最常见的机制是使用watched文件夹。

在AEM中配置Watched文件夹时，可以关联在文件放入Watched文件夹时执行的ECMA脚本或Java代码。 在本文中，我们将重点介绍ECMA脚本，该脚本将生成打印渠道文档并将其保存到文件系统。

监视文件夹配置和ECMA脚本是您在中导入的资产的一部分 [本教程的开头](introduction.md)

放入watched文件夹中的输入文件具有以下结构。 ECMA脚本读取帐号并为每个帐户生成打印渠道文档。

有关用于生成文档的ECMA脚本的更多详细信息， [请参阅本文](/help/forms/interactive-communications/generating-interactive-communications-print-document-using-api-tutorial-use.md)

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

要使用watched文件夹机制生成打印渠道文档，请执行以下步骤：

* [执行本文档中所述的步骤](/help/forms/adaptive-forms/service-user-tutorial-develop.md)

* 登录到crx并导航到/etc/fd/watchfolder/scripts/PrintPDF.ecma

* 确保interactiveCommunicationsDocument的路径指向要打印的正确文档。（第1行）
* 记下saveLocation（第2行）。您可以根据需要进行更改。
* 确保表单数据模型的输入参数已绑定到请求属性，并且其绑定值已设置为“accountnumber”。 请参阅下面的屏幕快照。
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

* 将xml文件放入C:\RenderPrintChannel\input

* 检查ECMA脚本中指定的保存位置中的pdf文件。

## 后续步骤

[在表单提交时打开代理ui](./opening-agent-ui-on-form-submission.md)