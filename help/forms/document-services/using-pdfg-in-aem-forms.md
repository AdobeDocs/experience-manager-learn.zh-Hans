---
title: 在AEM Forms使用PDFG
seo-title: 在AEM Forms使用PDFG
description: 演示使用AEM Forms创建PDF的拖放功能
seo-description: 演示使用AEM Forms创建PDF的拖放功能
uuid: ee54edb4-a7b1-42ed-81ea-cb6bef6cf97f
feature: pdf-generator
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 7f570f12-ce43-4da7-a249-ef6bd0fe48e5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 0%

---


# 在AEM Forms使用PDFG{#using-pdfg-in-aem-forms}

演示使用AEM Forms创建PDF的拖放功能

PDFG代表PDF生成。 这意味着您可以将各种文件格式转换为PDF。 最常见的是Microsoft Office文档。 PDFG自6.1起就成为AEM Forms的一部分。此处[列出PDFG API的javadoc](https://helpx.adobe.com/experience-manager/6-3/forms/using/aem-document-services-programmatically.html#PDFGeneratorService)

与本文关联的资源将允许您将MS office文档或JPG文件拖放到HTML页面的拖放区。 删除文档后，它将调用PDFG服务并将文档转换为PDF并将其保存到AEM服务器的文件系统中。

要安装演示资源，请执行以下步骤

1. 按此文档配置PDFG [](https://helpx.adobe.com/experience-manager/6-4/forms/using/install-configure-pdf-generator.html)。
1. 请按照与您的AEM Forms版本相关的相应文档操作。
1. [使用包管理器导入和安装与本文相关的资产。](assets/createpdfgdemov2.zip)
1. [在CRX中导航到post](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) .jsp
1. 根据您的首选项更改保存位置（第9行）
1. 保存更改。
1. 打开HTML [ 页面](http://localhost:4502/content/DocumentServices/CreatePDFG.html) ，拖放文件进行转换。
1. 将单词文件或jpg放入放置区。
1. 输入文档将转换为PDF并保存在与第4点中指定的位置中。

以下代码片段显示了PDFG服务将文件转换为PDF的用法

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```

