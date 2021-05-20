---
title: 为打印渠道文档创建两列布局
seo-title: 为打印渠道文档创建两列布局
description: 为打印渠道文档创建2列布局
seo-description: 为打印渠道文档创建2列布局
feature: 交互式通信
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 开发
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '241'
ht-degree: 2%

---


# 打印渠道文档中的两列布局

本文将重点介绍在打印渠道中创建2列布局所需的步骤。 用例是生成2页文档，其中第1页具有2列布局，第2页具有标准的1列布局。

以下是使用AEM Forms Designer创建2列布局时涉及的高级步骤。

* 在第1页的主控页面中创建2个内容区域
* 将2个内容区域命名为“leftcolumn”和“rightcolumn”
* 创建具有一个内容区域的第二个主控页面（这是默认内容）
* 选择“分页”选项卡（无标题子表单）（第1页）和（无标题子表单）（第2页），然后设置如下面屏幕快照中所示的属性。

![page1](assets/untitledsubform_paginationproperties.gif)

![page2](assets/untitled_subformpage2.gif)

设置分页属性后，我们可以在（无标题子表单）下添加子表单或目标区域（第1页）。

然后，我们可以向这些子表单或目标区域添加文档片段。 当左列已满时，内容将流向右列。

要在本地服务器上测试此功能，请下载与本文相关的资产。 向下滚动到此页面底部

* [使用包管理器下载并安装示例打印渠道文档](assets/print-channel-with-two-column-layout.zip)
* [预览打印渠道文档](http://localhost:4502/content/dam/formsanddocuments/2columnlayout/jcr:content?channel=print&amp;mode=preview&amp;dataRef=service%3A%2F%2FFnDTestData&amp;wcmmode=disabled)
