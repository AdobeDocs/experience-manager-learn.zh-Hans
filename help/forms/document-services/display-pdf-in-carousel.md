---
title: 显示多个PDF文档
description: 在自适应表单中循环浏览多个pdf文档。
version: 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
jira: KT-10292
exl-id: c1d248c3-8208-476e-b0ae-cab25575cd6a
last-substantial-update: 2021-10-12T00:00:00Z
duration: 74
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '320'
ht-degree: 1%

---

# 在传送中显示多个PDF文档

一个常见用例是在提交表单之前向表单填充器显示多个PDF文档以供审阅。

为了完成此用例，我们利用了 [Adobe PDF嵌入API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

[可在此处体验此示例的实时演示。](https://forms.enablementadobe.com/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)

已执行以下步骤以完成集成

## 创建自定义组件以显示多个PDF文档

创建了一个自定义组件(pdf-carousel)以在pdf文档之间循环

## 客户端库

已创建客户端库，以使用Adobe PDF Embed API显示PDF。 要显示的PDF在pdf轮盘组件中指定。

## 创建自适应表单

创建基于某些选项卡的自适应表单（此示例有3个选项卡）在前两个选项卡中添加一些自适应表单组件在第三个选项卡中添加pdf轮盘组件配置pdf轮盘组件，如下面的屏幕快照所示
![pdf-carousel](assets/pdf-carousel-af-component.png)

**嵌入PDFAPI密钥**  — 这是可用于嵌入PDF的键。 此密钥仅适用于localhost。 您可以创建 [您自己的密钥](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) 并将其与其他域相关联。

**指定PDF文档**  — 在此处，您可以指定希望在轮播中显示的pdf文档。


## 在服务器上部署示例

要在本地服务器上对此进行测试，请执行以下步骤：

1. [导入客户端库](assets/pdf-carousel-client-lib.zip) 到您的本地AEM实例中 [使用包管理器](http://localhost:4502/crx/packmgr/index.jsp)
1. [导入PDF传送组件](assets/pdf-carousel-component.zip) 到您的本地AEM实例中 [使用包管理器](http://localhost:4502/crx/packmgr/index.jsp)
1. [导入自适应表单](assets/adaptive-form-pdf-carousel.zip) 到您的本地AEM实例中 [使用包管理器](http://localhost:4502/crx/packmgr/index.jsp)
1. [导入要显示的示例PDF](assets/pdf-carousel-sample-documents.zip) 到您的本地AEM实例中 [使用assets文件上传链接](http://localhost:4502/assets.html/content/dam)
1. [预览自适应表单](http://localhost:4502/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)
1. 选项卡到“要审阅的文档”选项卡。 您应该会在轮盘组件中看到三个PDF文档。
