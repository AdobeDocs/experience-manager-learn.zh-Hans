---
title: 分隔工具栏的下一个和上一个按钮
description: 分隔工具栏按钮
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9291
exl-id: 1b55b6d2-3bab-4907-af89-c81a3b1a44cb
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# 为工具栏按钮留出空格

在AEM Forms的工具栏中添加“下一个”和“上一个”按钮时，这两个按钮默认会放置在一起。 您可能希望将“下一步”按钮推到工具栏的右上角，同时将上一个/后退按钮保留在左侧

![工具栏间距](assets/toolbar-spacing.png)


## 为工具栏设置样式

使用样式编辑器可以轻松完成上述用例。 将“上一个/下一个”按钮添加到工具栏后，请确保已从“编辑”菜单中选择“样式”图层。 选择样式模式后，选择工具栏以打开其样式设置属性表。 展开Dimension和位置部分，并确保您看到了所有资产。 设置以下属性
* Dimension和位置
   * 宽度：100%
   * 位置：相对

保存更改

## 设置“下一步”按钮的样式

选择“下一个”按钮，并确保打开“下一个”按钮（而不是“下一个”按钮文本）的样式属性表。 设置以下属性
* Dimension和位置
   * 位置：绝对顶部1px右侧1px
* 边框
   * 边框半径：4px（顶部、右侧、底部、左侧）

保存更改
