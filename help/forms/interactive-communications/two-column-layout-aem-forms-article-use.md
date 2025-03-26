---
title: 为打印渠道文档创建两个列布局
description: 为打印渠道文档创建2列布局
feature: Interactive Communication
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 416e3401-ba9f-4da3-8b07-2d39f9128571
last-substantial-update: 2019-07-07T00:00:00Z
duration: 44
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 0%

---

# 打印渠道文档中的两列布局

这篇简短文章将重点介绍在打印渠道中创建2列布局所需的步骤。 用例是生成两个页面文档，其中第1页具有2列布局，而第2页具有标准1列布局。

以下是使用AEM Forms Designer创建2列布局时涉及的高级步骤。

* 在页面1母版页中创建2个内容区域
* 将2个内容区域命名为“leftcolumn”和“rightcolumn”
* 创建一个具有一个内容区域的第二个母版页（默认）
* 选择分页选项卡（无标题子表单）（第1页）和（无标题子表单）（第2页），并设置属性，如下面的屏幕截图所示。

![page1](assets/untitledsubform_paginationproperties.gif)

![page2](assets/untitled_subformpage2.gif)

设置分页属性后，我们便可以在（无标题子表单）（第1页）下添加子表单或目标区域。

然后，我们可以向这些子表单或目标区域添加文档片段。 当左列已满时，内容将流向右列。

要在本地服务器上对此进行测试，请下载与本文相关的资产。 向下滚动至此页面的底部

* [使用包管理器下载并安装示例打印渠道文档](assets/print-channel-with-two-column-layout.zip)
* [预览打印渠道文档](http://localhost:4502/content/dam/formsanddocuments/2columnlayout/jcr:content?channel=print&amp;mode=preview&amp;dataRef=service%3A%2F%2FFnDTestData&amp;wcmmode=disabled)
