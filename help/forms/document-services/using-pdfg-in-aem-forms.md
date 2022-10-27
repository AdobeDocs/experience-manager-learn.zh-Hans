---
title: 在AEM Forms中使用PDFG
description: 演示使用AEM Forms创建PDF的拖放功能
feature: PDF Generator
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: bc79fcbf-b8b3-4d3a-9cd6-0bcd9321c7d5
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 2%

---

# 在AEM Forms中使用PDFG{#using-pdfg-in-aem-forms}

演示使用AEM Forms创建PDF的拖放功能

PDFG表示PDF生成。 这意味着您可以将多种文件格式转换为PDF。 最常见的是Microsoft Office文档。 PDFG自6.1起成为AEM Forms的一部分。
[此处列出了PDFG API的javadoc](https://www.adobe.io/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)

利用与本文关联的资产，可将MS Office文档或JPG文件拖放到HTML页面的拖放区域。 文档一旦被删除，就会调用PDFG服务，并将文档转换为PDF，然后保存在AEM Server的文件系统上。

要安装演示资产，请执行以下步骤

1. 按照本文档中所述配置PDFG [此处](https://helpx.adobe.com/cn/experience-manager/6-4/forms/using/install-configure-pdf-generator.html).
1. 请遵循与您的AEM Forms版本相关的相应文档。
1. [使用包管理器导入和安装与本文相关的资产。](assets/createpdfgdemov2.zip)
1. [导航到post.jsp](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) 在CRX中
1. 根据您的首选项更改保存位置（第9行）
1. 保存更改。
1. 打开 [html页面](http://localhost:4502/content/DocumentServices/CreatePDFG.html) ，用于拖放要转换的文件。
1. 将单词文件或jpg放入拖放区域。
1. 输入文档将转换为PDF并保存在与第4点中指定的相同位置。

以下代码片段显示了PDFG服务在将文件转换为PDF时的用法

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```
