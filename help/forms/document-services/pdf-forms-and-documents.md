---
title: 了解不同类型的PDF forms和文档
description: PDF实际上是一系列文件格式，本文介绍了对表单开发人员而言重要且相关的PDF类型。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: 6.4, 6.5
feature: PDF Generator
kt: 7071
topic: Development
exl-id: ffa9d243-37e5-420c-91dc-86c73a824083
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '1277'
ht-degree: 0%

---

# PDF

可移植文档格式(PDF)实际上是一系列文件格式，本文详细介绍了与表单开发人员最相关的格式。 不同PDF类型的许多技术细节和标准都在不断发展和变化。 其中一些格式和规范是国际标准化组织(ISO)标准，有些是Adobe拥有的特定知识产权。

本文将向您展示如何创建各种类型的PDF。 它有助于您了解如何以及为何使用每个插件。 所有这些类型在主要客户端工具(Adobe Acrobat DC)中最适合用于查看和使用PDF。

以下是Acrobat DC中的PDF/A文件示例。

![Pdfa](assets/pdfa-file-in-acrobat.png)

示例文件可以是 [从此处下载](assets/pdf-file-types.zip)

## XML Forms架构PDF(XFAPDF)

Adobe使用术语XFAPDF表单来指代您使用AEM Forms Designer创建的交互式动态Forms。 使用Designer创建的Forms和文件均基于Adobe的XML Forms架构(XFA)。 从很多方面讲，XFAPDF文件格式比传统HTML文件更接近PDF文件。 例如，以下代码显示了XFAPDF文件中简单文本对象的外观。

![文本字段](assets/text-field.JPG)

XFA Forms基于XML。 这种结构良好且灵活的格式使AEM Forms Server能够将您的Designer文件转换为不同的格式，包括传统PDF、PDF/A和HTML。 通过选择布局编辑器的“XML源”选项卡，您可以在Designer中查看Forms的完整XML结构。 您可以在AEM Forms Designer中创建静态和动态XFA Forms。

## 静态PDF

静态XFAPDF forms布局在运行时从未更改，但它们对于用户而言可以是交互式的。 以下是静态XFAPDF forms的一些优势：

* 静态XFAPDF forms布局在运行时从未更改，但它们对于用户而言可以是交互式的。
* 静态Forms支持Acrobat的注释和标记工具。
* 静态Forms允许您导入和导出Acrobat注释。
* 静态Forms支持字体子设置，这是一种可在AEM Forms服务器上完成的技术。
* 静态Forms可以使用现代浏览器附带的内置PDF查看器进行渲染。

>[!NOTE]
>
> 您可以使用AEM Forms Designer通过将XDP保存为“静态PDF表单”来创建静态Adobe



### 动态Forms

动态XFAPDF可以在运行时更改其布局，因此不支持注释和标记功能。 但是，动态XFAPDF具有以下优势：

* 动态表单支持可更改表单布局和分页的客户端脚本。 例如，如果将Purchase Order.xdp另存为动态表单，则它将扩展并分页，以容纳无限数量的数据
* 动态表单在运行时支持表单的所有属性，而静态表单仅支持子集

>[!NOTE]
>
> 您可以使用AEM Forms Designer通过将XDP另存为Adobe动态XML表单来创建动态PDF

>[!NOTE]
>
> 无法使用现代浏览器内建的pdf查看器来渲染动态表单。

### PDF文件(传统PDF)

认证文档为PDF文档和Forms接收者提供了对其真实性和完整性的附加保证。

最流行、最普遍的PDF格式是传统的PDF文件。 创建传统PDF文件的方法有很多，包括使用Acrobat和许多第三方工具。 Acrobat提供了以下所有方法来创建传统PDF文件。 如果未安装Acrobat，则可能在计算机上看不到这些选项。

* 通过捕获桌面应用程序的打印流：选择创作应用程序的“打印”命令，然后选择Adobe PDF打印机图标。 您将创建文档的PDF文件，而不是文档的打印副本
* 通过将Acrobat PDFMaker插件与Microsoft Office应用程序结合使用：安装Acrobat时，它会向Microsoft Office应用程序添加一个Adobe PDF菜单，并向Office功能区添加一个图标。 您可以使用这些添加的功能直接在Microsoft Office中创建PDF文件
* 通过使用Acrobat Distiller将Postscript和封装的Postscript(EPS)文件转换为PDF:Distiller通常用于印刷发布和其他工作流程，这些工作流程需要从Postscript格式转换为PDF格式
* 在引擎罩下，传统PDF与XFAPDF截然不同。 它的XML结构不相同，并且由于它是通过捕获文件的打印流创建的，因此传统PDF是静态的只读文件。

认证文档为PDF文档和表单接收者提供对其真实性和完整性的附加保证。

### Acroforms

Acroforms是Adobe较旧的交互式表单技术；它们的日期可追溯到Acrobat版本3。 Adobe提供 [Acrobat Forms API参考](assets/FormsAPIReference.pdf)，日期为2003年5月，以提供此技术的技术详细信息。 Acroforms是以下项目的组合：

* 定义表单静态布局和图形的传统PDF。
* 使用Adobe Acrobat程序的表单工具将交互式表单字段固定到顶部。 这些表单工具只是AEM Forms Designer中可用功能的一小部分。

### PDF/A(用于存档的PDF)

PDF/A(PDF归档)以传统PDF的文档存储优势为基础，提供了许多可增强长期归档的具体细节。 传统的PDF文件格式为长期文档存储提供了许多好处。 PDF的紧凑性有助于轻松传输和节约空间，其结构良好的特性支持强大的索引和搜索功能。 传统PDF对元数据有广泛的支持，而PDF在支持不同计算机环境方面有着悠久的历史。

与PDF一样，PDF/A是ISO标准规范。 它是由一个特别工作组开发的，其中包括AIIM（信息和图像管理协会）、NPES（国家印刷设备协会）和美国法院行政办公室。 由于PDF/A规范的目标是提供长期存档格式，因此会忽略许多PDF功能，因此文件可以自包含。 以下是关于规范的一些要点，这些要点可增强PDF/A文件的长期重现性：

* 所有内容都必须包含在文件中，并且不能依赖外部源，如超链接、字体或软件程序。
* 所有字体都必须嵌入，并且它们必须是具有无限使用许可的电子文档字体。
* 不允许使用JavaScript
* 不允许透明
* 不允许加密
* 不允许音频和视频内容
* 色彩空间必须以与设备无关的方式定义
* 所有元数据都必须遵循特定标准

### 查看PDF/文件

样例文件中的两个文件是从同一个Microsoft Word文件创建的。 一个创建为传统PDF，另一个创建为PDF/A文件。 在Acrobat Professional中打开以下两个文件：

* simpleWordFile.pdf
* simpleWordFilePDFA.pdf

尽管文档“外观相同”，但PDF/A文件会在顶部以蓝色条打开，表示您正在PDF/A模式下查看此文档。 此蓝色条是Acrobat的文档消息栏，在打开某些类型的PDF文件时，您会看到该消息栏。

![Pdf-img](assets/pdfa-message.png)

文档消息栏包含说明（可能还包含按钮），可帮助您完成任务。 它采用颜色编码，当您打开特殊类型的PDF(如此PDF/A文件)以及经过认证和数字签名的PDF时，您会看到蓝色。 当您参与PDF forms审阅时，条状图变为紫色，变为黄色。

>[!NOTE]
>
> 如果单击启用编辑，则会使此文档不符合PDF/A规范。
