---
title: 了解不同类型的PDF forms和文档
description: PDF实际上是一系列文件格式，本文描述了对表单开发人员来说重要且相关的PDF类型。
solution: Experience Manager Forms
product: aem
type: 文档
role: 开发人员
level: 初学者，中级
version: 6.3,6.4,6.5
feature: 文档服务
topic: 开发
kt: 7071
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1301'
ht-degree: 0%

---


# PDF格式

可移植文档格式(PDF)实际上是一系列文件格式，本文详细介绍了最适合表单开发人员的格式。 许多不同PDF类型的技术细节和标准都在不断发展变化。 其中一些格式和规范是国际标准化组织(ISO)标准，有些是Adobe拥有的特定知识产权。

本文向您介绍如何创建各种类型的PDF。 它将帮助您了解如何和为何使用每个组件。 所有这些类型在用于查看和处理PDF的主客户端工具 — Adobe Acrobat DC中最有效。

这是Acrobat DC中的PDF/A文件示例。
![pdfa](assets/pdfa-file-in-acrobat.png)

可从此处下载示例文件[](assets/pdf-file-types.zip)

## XFA PDF

Adobe使用术语“PDF表单”指您使用AEM Forms Designer创建的交互式和动态表单。 请务必注意，还有另一种类型的PDF表单，称为Acroform，它与您在AEM Forms Designer中创建的PDF forms不同。 您使用设计器创建的表单和文件基于Adobe的XML Forms体系架构(XFA)。 在许多方面，XFA PDF文件格式比传统PDF文件更接近HTML文件。 例如，下面的代码向您显示简单文本对象在XFA PDF文件中的外观。

![文本字段](assets/text-field.JPG)

如您所见，XFA表单是基于XML的。 这种结构良好且灵活的格式使AEM Forms服务器能够将您的Designer文件转换为不同格式，包括传统的PDF、PDF/A和HTML。 通过选择布局编辑器的“XML源”选项卡，您可以在设计器中查看表单的完整XML结构。 您可以在AEM Forms Designer中创建静态和动态XFA表单。

## 静态PDF

静态XFAPDF forms在运行时不会更改其布局，但是对于用户而言，它们可以是交互式的。 以下是静态XFAPDF forms的几个优势：

* 静态XFAPDF forms在运行时不会更改其布局，但是对于用户而言，它们可以是交互式的。
* 静态表单支持Acrobat的注释和标记工具。
* 静态表单允许您导入和导出Acrobat注释。
* 静态表单支持字体子设置，这是一种可在AEM Forms服务器上完成的技术。
* 使用新式浏览器附带的内置pdf查看器可以渲染静态表单。

>[!NOTE]
> 可以使用AEM Forms Designer将XDP另存为Adobe静态PDF表单，从而创建静态PDF

## 动态Forms

动态XFA PDF可以在运行时更改其布局，因此不支持注释和标记功能。 但是，动态XFA PDF可以优惠以下优势：

* 动态表单支持客户端脚本，这些脚本可更改表单的布局和分页。 例如，如果将Purchase Order.xdp另存为动态表单，它将扩展并分页，以容纳无限数量的数据
* 动态表单在运行时支持表单的所有属性，而静态表单只支持子集


>[!NOTE]
> 可以使用AEM Forms Designer将XDP另存为Adobe动态XML表单，从而创建动态PDF

>[!NOTE]
> 无法使用现代浏览器内置的pdf查看器呈现动态表单。


## PDF文件（传统PDF）

最流行、最普及的PDF格式是传统的PDF文件。 创建传统PDF文件有多种方式，包括使用Acrobat和许多第三方工具。 Acrobat提供了以下创建传统PDF文件的所有方法。 如果未安装Acrobat，您可能在计算机上看不到这些选项。

* 通过捕获桌面应用程序的打印流：选择创作应用程序的“打印”命令，然后选择Adobe PDF打印机图标。 您将创建文档的PDF文件，而不是打印的文档
* 将Acrobat PDFMaker插件与Microsoft Office应用程序一起使用：安装Acrobat时，它会向Microsoft Office应用程序添加一个Adobe PDF菜单，并向Office功能区添加一个图标。 您可以使用这些添加的功能直接在Microsoft Office中创建PDF文件
* 使用Acrobat Distiller将Postscript和封装的Postscript(EPS)文件转换为PDF:Distiller通常用于印刷出版和其他需要从Postscript格式转换为PDF格式的工作流
* 在内部，传统PDF与XFA PDF非常不同。 它的XML结构不相同，而且由于它是通过捕获文件的打印流创建的，因此传统的PDF是静态的只读文件。

认证文档为PDF文档和表单收件人提供了对其真实性和完整性的额外保证。

## Acroforms

Acroforms是Adobe较旧的交互式表单技术；它们的历史可以追溯到Acrobat 3版。 Adobe提供日期为2003年5月的[Acrobat Forms API参考](assets/FormsAPIReference.pdf)，以提供此技术的技术详细信息。 Acroforms是
以下项目：

* 定义表单静态布局和图形的传统PDF。
* 使用Adobe Acrobat 项目的表单工具将交互式表单域栓在顶部。 请注意，这些表单工具只是AEM Forms Designer中可用功能的一小部分。

## PDF/A（用于存档的PDF）

PDF/A(PDF for Archives)以传统PDF的文档存储优势为构建基础，并包含许多可增强长期归档的特定详细信息。 传统的PDF文件格式优惠了长期文档存储的许多优势。 PDF的紧凑特性简化了传输并节省了空间，其结构良好的特性支持强大的索引和搜索功能。 传统PDF对元数据有广泛的支持，而PDF在支持不同计算机环境方面有着悠久的历史。

与PDF一样，PDF/A是ISO标准规范。 它由一支任务部队开发，其中包括AIIM（信息和图像管理协会）、NPES（国家印刷设备协会）和美国法院行政办公室。 由于PDF/A规范的目标是提供长期存档格式，因此会忽略许多PDF功能，因此文件可以自包含。 以下是有关增强PDF/A文件长期可再现性的规范的一些要点：

* 文件中必须包含所有内容，并且不能依赖外部源，如超链接、字体或软件项目。
* 所有字体必须嵌入，并且它们必须是具有电子文档无限使用许可证的字体。
* 不允许使用JavaScript
* 不允许透明
* 不允许加密
* 不允许音频和视频内容
* 色彩空间必须以与设备无关的方式定义
* 所有元数据必须符合某些标准

## 查看PDF/A文件

示例文件中的两个文件是从同一个Microsoft Word文件创建的。 一个创建为传统PDF，另一个创建为PDF/A文件。 在Acrobat Professional中打开以下两个文件：

simpleWordFile.pdf
simpleWordFilePDFA.pdf

尽管文档看起来相同，但PDF/A文件在顶部以蓝色条打开，表示您正在PDF/A模式下查看此文档。 此蓝色条是Acrobat的文档消息栏，打开某些类型的PDF文件时，您将看到该消息栏。

![pdf-img](assets/pdfa-message.png)

文档消息栏包含说明，可能还包含按钮，可帮助您完成任务。 它是彩色编码的，当您打开特殊类型的PDF（如此PDF/A文件）以及经过认证和数字签名的PDF时，您会看到蓝色。 当您参与PDF审阅时，条状将变为PDF forms的紫色和黄色。

>[!NOTE]
> 如果单击“启用编辑”，您将使此文档不再符合PDF/A规范。




