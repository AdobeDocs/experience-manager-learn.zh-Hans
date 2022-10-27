---
title: 使用项目加载路径填充下拉列表
description: 配置和填充下拉列表以从crx节点读取值
feature: Adaptive Forms
version: 6.4,6.5
kt: 10961
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-20T00:00:00Z
thumbnail: item-load.jpg
source-git-commit: e1c16ff347f5f398c7bc47233049427eeffa2aab
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 0%

---

# AEM Forms中的项目加载属性

使用项目加载路径属性配置和填充下拉列表。
项目加载路径字段允许作者提供一个URL，从中加载下拉列表中可用的选项。
要在crx中创建此类节点，请按照以下所述步骤操作：
* 登录到crx
* 创建一个名为assets的节点（您可以根据需要命名此节点），键入sling:folder（位于内容下）。
* 保存
* 单击新创建的资产节点，并设置其属性，如下所示
* 您需要创建名为assettypes的字符串类型属性（您可以根据需要将其命名）。确保该属性是多值属性。 提供所需的值并进行保存。
   ![项目加载路径](assets/item-load-path-crx.png)

要在下拉列表中加载这些值，请在项目加载路径属性中提供以下路径  **/content/assets/assettypes**

示例包可以是 [从此处下载](assets/item-load-path-package.zip)
