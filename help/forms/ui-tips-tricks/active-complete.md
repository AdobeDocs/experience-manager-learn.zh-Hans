---
title: 设置左侧导航选项卡的样式，其中包含图标
description: 添加图标以指示活动和已完成的选项卡
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9359
exl-id: f7c1f991-0486-4355-8502-cd5b038537e3
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 15%

---

# 添加图标以指示活动和已完成的选项卡

当您具有带有左选项卡导航的自适应表单时，您可能希望显示图标以指示选项卡的状态。 例如，您希望显示一个图标来指示活动选项卡，并显示一个图标来指示已完成的选项卡，如以下屏幕截图所示。

![工具栏间距](assets/active-completed.png)

## 创建自适应表单

基于“基本”模板和“画布3.0”主题的简单自适应表单用于创建示例表单。
的 [本文中使用的图标](assets/icons.zip) 可从此处下载。


## 设置默认状态的样式

在编辑模式下打开表单确保您位于样式层中，并选择任意选项卡（例如“常规”选项卡）。
打开选项卡的样式编辑器时，您处于默认状态，如下面的屏幕快照所示
![navigation-tab](assets/navigation-tab.png)

为默认状态设置CSS属性，如下所示 |类别 |属性名称 |属性值 | |:—|:—|:—| |Dimension和位置 |宽度 | 50px | |文本 |字体粗细|粗体 | |文本 |颜色 | #FFF | |文本 |行高| 3 | |文本 |文本对齐 |左 | |背景|颜色 | #056dae |

保存更改

## 设置活动状态的样式

确保您处于“活动”状态，并设置以下CSS属性的样式

| 类别 | 属性名称 | 属性值 |
|:---|:---|:---|
| Dimension和位置 | 宽度 | 50px |
| 文本 | 字体粗细 | 粗体 |
| 文本 | 颜色 | #FFF |
| 文本 | 行高 | 3 |
| 文本 | 文本对齐 | 左 |
| 背景 | 颜色 | #056dae |

设置背景图像的样式，如下面屏幕快照所示

保存更改。



![活动状态](assets/active-state.png)

## 已访问状态的样式

确保您处于已访问状态，并设置以下属性的样式

| 类别 | 属性名称 | 属性值 |
|:---|:---|:---|
| Dimension和位置 | 宽度 | 50px |
| 文本 | 字体粗细 | 粗体 |
| 文本 | 颜色 | #FFF |
| 文本 | 行高 | 3 |
| 文本 | 文本对齐 | 左 |
| 背景 | 颜色 | #056dae |

设置背景图像的样式，如下面屏幕快照所示


![访问状态](assets/visited-state.png)

保存更改

预览表单并测试图标是否按预期工作。
