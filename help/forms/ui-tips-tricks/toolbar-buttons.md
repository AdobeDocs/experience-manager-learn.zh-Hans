---
title: 将工具栏的下一个和上一个按钮隔开
description: 为工具栏按钮设置空格
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9270
source-git-commit: 96b78ff5056bd9c2be39fb2cf21b4f92863af089
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# 将工具栏按钮放在空间

在AEM Forms的工具栏中添加“下一步”和“上一步”按钮时，默认情况下，按钮会彼此相邻放置。 您可能希望在左侧保留“上一步/后退”按钮的同时，将“下一步”按钮按到工具栏的最右侧

![工具栏间距](assets/toolbar-spacing.png)


## 设置工具栏的样式

使用样式编辑器可以轻松地完成上述用例。 在工具栏中添加“前/后”按钮后，请确保已从编辑菜单中选择“样式”层。 选择样式模式后，选择工具栏以打开其样式属性工作表。 展开“Dimension和位置”部分，并确保看到所有属性。 设置以下属性
* Dimension和位置
   * 宽度：100%
   * 位置：相对

保存更改

## 设置“下一步”按钮的样式

选择“下一步”按钮，并确保打开下一个按钮的样式属性工作表（而不是下一个按钮文本）。 设置以下属性
* Dimension和位置
   * 位置：绝对上1像素右1像素
* 边框
   * 边框半径：4px（上、右、下、左）

保存更改
