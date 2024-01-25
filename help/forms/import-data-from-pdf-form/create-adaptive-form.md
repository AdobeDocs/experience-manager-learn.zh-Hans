---
title: 创建架构
description: 根据需要导入自适应表单中的数据创建架构
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-14196
exl-id: b286c3e9-70df-46e8-b0bc-21599ab1ec06
duration: 46
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 1%

---

# 简介

第一步是根据将用于填充自适应表单的数据创建架构。

## XFA基于架构

使用架构创建自适应表单

## XFA不基于架构

* 在AEM Forms Designer中打开XDP。
* 单击“文件” | 表单属性 | 预览。
* 单击生成预览数据。
* 单击“生成”。
* 提供有意义的文件名，例如 `form-data.xml`

您可以使用任何免费在线工具来 [生成XSD](https://www.freeformatter.com/xsd-generator.html) 来自上一步中生成的xml数据。

根据上一步中的架构创建自适应表单。

>[!NOTE]
>始终建议检查在提交自适应表单时生成的数据。 这样您就可以很好地了解需要与自适应表单合并的数据的XML格式。

从自适应表单提交的数据
![submitted-data](./assets/af-submitted-data.png)

从PDF导出的数据
![exported-data](./assets/exported-data.png)

对于导出的数据，您必须提取 **_topmostSubform_** 保留适当命名空间的节点，以便成功将数据与自适应表单合并。

## 后续步骤

[创建OSGi服务](./create-osgi-service.md)
