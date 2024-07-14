---
title: 导航嵌套面板
description: 导航嵌套面板
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9335
exl-id: c60d019e-da26-4f67-8579-ef707e2348bb
last-substantial-update: 2019-07-07T00:00:00Z
duration: 264
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '247'
ht-degree: 0%

---

# 具有多个面板的导航选项卡

当表单具有左侧导航选项卡并且其中一个选项卡具有多个面板时，您可能希望隐藏子面板的标题，并且仍然能够在这些选项卡的选项卡和子面板之间导航

## 创建自适应表单

创建具有以下结构的自适应表单。 根面板具有子面板，这些子面板在左侧显示为选项卡。 这些“**选项卡**”中的一些具有其他子面板。 例如，“家庭”选项卡有两个名为“配偶”和“子女”的子面板。

在FormContainer下还添加了工具栏，其中包含“上一个”和“下一个”按钮

![工具栏间距](assets/multiple-panels.png)



此表单的默认行为是显示左侧的所有面板，然后单击下一步按钮从一个选项卡导航到另一个选项卡。

要更改此默认行为，我们需要执行以下操作

>[!VIDEO](https://video.tv.adobe.com/v/338369?quality=12&learn=on)


使用代码编辑器在&#x200B;**下一步**&#x200B;按钮的单击事件中添加以下代码

```javascript
window.guideBridge.setFocus(null, 'nextItemDeep', true);
```

使用代码编辑器在&#x200B;**Prev**&#x200B;按钮的单击事件中添加以下代码

```javascript
window.guideBridge.setFocus(null, 'prevItemDeep', true);
```

上述代码将帮助您在每个选项卡的选项卡和子面板之间导航。

## 隐藏子面板标题

使用样式编辑器可隐藏选项卡子面板的标题。

>[!VIDEO](https://video.tv.adobe.com/v/338370?quality=12&learn=on)

>[!NOTE]
>
>本文中介绍的功能在最后一个选项卡中不起作用。 例如，如果“地址”选项卡具有子面板，则此功能将无效。
