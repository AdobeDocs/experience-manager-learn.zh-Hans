---
title: 浏览嵌套的面板
description: 浏览嵌套的面板
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9335
exl-id: c60d019e-da26-4f67-8579-ef707e2348bb
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '247'
ht-degree: 0%

---

# 包含多个面板的导航选项卡

当表单具有左侧导航选项卡，并且其中一个选项卡具有多个面板时，您可能希望隐藏子面板的标题，并且仍然能够在这些选项卡的选项卡和子面板之间导航

## 创建自适应表单

创建具有以下结构的自适应表单。 根面板具有子面板，这些面板在左侧显示为选项卡。 其中一些“**选项卡**“ ”具有其他子面板。 例如，“家庭”选项卡有两个名为“配偶”和“子女”的子面板。

表单容器下还添加了一个工具栏，其中包含上一个和下一个按钮

![工具栏间距](assets/multiple-panels.png)



此表单的默认行为是在左侧显示所有面板，然后单击下一个按钮从一个选项卡导航到另一个选项卡。

要更改此默认行为，我们需要执行以下操作

>[!VIDEO](https://video.tv.adobe.com/v/338369?quality=9&learn=on)


将以下代码添加到 **下一个** 按钮

```javascript
window.guideBridge.setFocus(null, 'nextItemDeep', true);
```

将以下代码添加到 **上一版** 按钮

```javascript
window.guideBridge.setFocus(null, 'prevItemDeep', true);
```

上述代码将帮助您在每个选项卡的选项卡和子面板之间导航。

## 隐藏子面板标题

使用样式编辑器隐藏选项卡子面板的标题。

>[!VIDEO](https://video.tv.adobe.com/v/338370?quality=9&learn=on)

>[!NOTE]
>
>本文中描述的功能在最后一个选项卡中不起作用。 例如，如果“地址”选项卡具有子面板，则此功能将不起作用。
