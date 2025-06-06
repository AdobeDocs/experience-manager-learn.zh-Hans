---
title: 使用项加载路径填充下拉列表
description: 配置并填充下拉列表以从crx节点读取值
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-10961
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-20T00:00:00Z
thumbnail: item-load.jpg
exl-id: 89c486c8-95c3-4cd4-bf8e-a1b3558f17d6
duration: 34
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 0%

---

# AEM Forms中的项目加载属性

使用项目加载路径属性配置和填充下拉列表。
利用项目加载路径字段，作者可以提供URL，以从中加载下拉列表中可用的选项。
要在crx中创建此类节点，请执行以下步骤：
* 登录crx
* 创建名为assets的节点（您可以根据需要命名此节点），在content下键入sling：folder。
* 保存
* 单击新创建的资源节点，并设置其属性，如下所示
* 您将需要创建一个名为assettypes的String类型的属性（您可以根据需要进行命名）。请确保该属性为多值。 提供所需的值并保存。
  ![项加载路径](assets/item-load-path-crx.png)

要在下拉列表中加载这些值，请在项加载路径属性&#x200B;**/content/assets/assettypes**&#x200B;中提供以下路径

可从此处[&#128279;](assets/item-load-path-package.zip)下载示例包
