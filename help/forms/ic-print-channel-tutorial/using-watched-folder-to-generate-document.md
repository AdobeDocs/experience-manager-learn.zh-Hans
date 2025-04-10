---
title: 使用Watched文件夹生成打印渠道文档
description: 这是为打印渠道创建第一个交互式通信文档的多步教程的第10部分。 在本部分中，我们将使用watched文件夹机制生成打印渠道文档。
feature: Interactive Communication
doc-type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
contentOwner: gbedekar
discoiquuid: 23fbada3-d776-4b77-b381-22d3ec716ae9
topic: Development
role: Developer
level: Beginner
exl-id: 9bb05c94-2a7b-4149-b567-186eb08b1c66
duration: 70
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 0%

---

# 使用Watched文件夹生成打印渠道文档

在本部分中，我们将使用watched文件夹机制生成打印渠道文档。

创建和测试打印渠道文档后，我们需要一种机制以批量模式或按需生成这些文档。 通常，此类文档以批处理模式生成，最常见的机制是使用watched文件夹。

在AEM中配置watched文件夹时，您会关联ECMA脚本或Java代码，在将文件拖放到watched文件夹中时执行这些脚本或Java代码。 在本文中，我们将重点介绍ECMA脚本，该脚本将生成打印渠道文档并将其保存到文件系统。

观察文件夹配置和ECMA脚本是您在本教程[开头导入的资产的一部分](introduction.md)

放入watched文件夹中的输入文件具有以下结构。 ECMA脚本读取这些帐户编号并为每个帐户生成打印渠道文档。

有关用于生成文档的ECMA脚本的详细信息，[请参阅本文](/help/forms/interactive-communications/generating-interactive-communications-print-document-using-api-tutorial-use.md)

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

* [执行本文档中提到的步骤](/help/forms/adaptive-forms/service-user-tutorial-develop.md)

* 登录到crx并导航到/etc/fd/watchfolder/scripts/PrintPDF.ecma

* 确保interactiveCommunicationsDocument的路径指向要打印的正确文档。（第1行）
* 记下saveLocation（第2行）。您可以根据需要进行更改。
* 确保表单数据模型的输入参数已绑定到请求属性，并且其绑定值设置为“accountnumber”。 请参阅下面的屏幕截图。
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

[在提交表单时打开代理UI](./opening-agent-ui-on-form-submission.md)