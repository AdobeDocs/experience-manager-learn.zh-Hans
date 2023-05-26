---
title: 为打印渠道文档创建两个列布局
seo-title: Creating two column layouts for print channel documents
description: 为打印渠道文档创建2列布局
seo-description: Create 2 column layouts for print channel document
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 416e3401-ba9f-4da3-8b07-2d39f9128571
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# 打印渠道文档中的两列布局

这篇简短文章将重点介绍在打印渠道中创建2列布局所需的步骤。 用例是生成两个页面文档，其中页面1具有2列布局，页面2具有标准1列布局。

以下是使用AEM Forms Designer创建2列布局所涉及的高级步骤。

* 在第1页主控页面中创建2个内容区域
* 将2个内容区域命名为“leftcolumn”和“rightcolumn”
* 创建一个包含一个内容区域的第二个主控页面（默认）
* 选择分页选项卡（无标题子表单）（第1页）和（无标题子表单）（第2页），并设置属性，如下面的屏幕截图所示。

![page1](assets/untitledsubform_paginationproperties.gif)

![page2](assets/untitled_subformpage2.gif)

设置分页属性后，我们便可以在（无标题子表单）（第1页）下添加子表单或目标区域。

然后，我们可以将文档片段添加到这些子表单或目标区域。 当左列已满时，内容将流向右列。

要在本地服务器上对此进行测试，请下载与本文相关的资产。 向下滚动至此页面的底部

* [使用包管理器下载并安装示例打印渠道文档](assets/print-channel-with-two-column-layout.zip)
* [预览打印渠道文档](http://localhost:4502/content/dam/formsanddocuments/2columnlayout/jcr:content?channel=print&amp;mode=preview&amp;dataRef=service%3A%2F%2FFnDTestData&amp;wcmmode=disabled)
