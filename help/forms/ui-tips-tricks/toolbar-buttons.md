---
title: 将工具栏的下一个和上一个按钮隔开
description: 将工具栏按钮隔开
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9291
exl-id: 1b55b6d2-3bab-4907-af89-c81a3b1a44cb
last-substantial-update: 2019-07-07T00:00:00Z
duration: 39
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 0%

---

# 与工具栏按钮隔开

在AEM Forms的工具栏中添加“下一个”和“上一个”按钮时，默认情况下，这两个按钮会放置在一起。 您可能希望将“下一步”按钮推到工具栏的右上角，同时保留左边的“上一个/后一个”按钮

![工具栏间距](assets/toolbar-spacing.png)


## 设置工具栏样式

使用样式编辑器，可以轻松完成上述用例。 将“上一个/下一个”按钮添加到工具栏后，请确保已从“编辑”菜单中选择“样式”图层。 选择样式模式后，选择工具栏以打开其样式设置属性表。 展开“维度和位置”部分，并确保您看到了所有属性。 设置以下属性
* 维度和位置
   * 宽度：100%
   * 位置：相对

保存更改

## 设置“下一步”按钮的样式

选择“下一步”按钮，并确保打开“下一步”按钮（而不是“下一步”按钮文本）的样式属性表。 设置以下属性
* 维度和位置
   * 位置：绝对值顶部1px右侧1px
* 边框
   * 边框半径：4像素（上、右、下、左）

保存更改
