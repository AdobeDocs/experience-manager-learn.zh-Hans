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

PDFG表示PDF生成。 这意味着您可以将各种文件格式转换为PDF。 最常见的文档是Microsoft Office文档。 PDFG从6.1开始一直是AEM Forms的一部分。
[此处列出了适用于PDFG API的javadoc](https://www.adobe.io/experience-manager/reference-materials/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)

与本文相关的资源将允许您将MS Office文档或JPG文件拖放到HTML页的拖放区域中。 文档一旦被删除，就会调用PDFG服务，将文档转换为PDF并保存在AEM Server的文件系统中。

要安装演示资产，请执行以下步骤

1. 按本文档所述配置PDFG [此处](https://helpx.adobe.com/cn/experience-manager/6-4/forms/using/install-configure-pdf-generator.html).
1. 请遵循与您的AEM Forms版本相关的相应文档。
1. [使用包管理器导入和安装与本文相关的资产。](assets/createpdfgdemov2.zip)
1. [导航到post.jsp](http://localhost:4502/apps/AemFormsSamples/components/createPDF/POST.jsp) 在您的CRX中
1. 根据您的喜好更改保存位置（第9行）
1. 保存更改。
1. 打开 [html页面](http://localhost:4502/content/DocumentServices/CreatePDFG.html) 用于拖放文件以进行转换。
1. 将word文件或jpg拖放到拖放区域中。
1. 输入文档被转换为PDF并保存在点4中指定的相同位置。

以下代码段显示了PDFG服务将文件转换为PDF的用法

```java
com.adobe.pdfg.service.api.GeneratePDFService pdfService = sling.getService(com.adobe.pdfg.service.api.GeneratePDFService.class);
System.out.println("Got PDF Service");
java.util.Map map = pdfService.createPDF(uploadedDocument,fileName,"","Standard","No Security", null, null);
```
