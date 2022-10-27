---
title: 显示多个PDF文档
description: 在自适应表单中循环浏览多个PDF文档。
version: 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
kt: 10292
exl-id: c1d248c3-8208-476e-b0ae-cab25575cd6a
last-substantial-update: 2021-10-12T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 4%

---

# 以轮播方式显示多个PDF文档

一个常见用例是，在提交表单之前，向表单填充器显示多个PDF文档以供审阅。

为了完成此用例，我们使用 [Adobe PDF嵌入API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

[您可以在此体验此示例的实时演示。](https://forms.enablementadobe.com/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)

已执行以下步骤来完成集成

## 创建自定义组件以显示多个PDF文档

创建了自定义组件(pdf-carousel)以循环查看pdf文档

## 客户端库

创建了客户端库，以使用Adobe PDF嵌入API显示PDF。 要显示的PDF在pdf-carousel组件中指定。

## 创建自适应表单

创建基于某些选项卡的自适应表单（此示例有3个选项卡）在前两个选项卡中添加一些自适应表单组件在第三个选项卡中添加pdf轮播组件配置pdf轮播组件，如以下屏幕截图所示
![pdf-carousel](assets/pdf-carousel-af-component.png)

**嵌入PDFAPI密钥**  — 这是可用于嵌入PDF的键。 此键将仅适用于localhost。 您可以创建 [您自己的密钥](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) 并将其与其他域关联。

**指定PDF文档**  — 在此，您可以指定要在轮播中显示的pdf文档。


## 在服务器上部署示例

要在本地服务器上测试此功能，请执行以下步骤：

1. [导入客户端库](assets/pdf-carousel-client-lib.zip) 到本地AEM实例 [使用包管理器](http://localhost:4502/crx/packmgr/index.jsp)
1. [导入pdf轮播组件](assets/pdf-carousel-component.zip) 到本地AEM实例 [使用包管理器](http://localhost:4502/crx/packmgr/index.jsp)
1. [导入自适应表单 ](assets/adaptive-form-pdf-carousel.zip) 到本地AEM实例 [使用包管理器](http://localhost:4502/crx/packmgr/index.jsp)
1. [导入要显示的示例PDF](assets/pdf-carousel-sample-documents.zip) 到本地AEM实例 [使用assets文件上传链接](http://localhost:4502/assets.html/content/dam)
1. [预览自适应表单](http://localhost:4502/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)
1. 选项卡，打开要审阅的文档选项卡。 您应会在轮播组件中看到三个PDF文档。
