---
title: 在AEM FormsCloud Service中使用垂直选项卡
description: 使用垂直制表符创建自适应表单
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
thumbnail: 331891.jpg
badgeVersions: label="AEM Formsas a Cloud Service" before-title="false"
jira: KT-16023
exl-id: c5bbd35e-fd15-4293-901e-c81faf6025f9
source-git-commit: ba744f95f8d1f0b982cd5430860f0cb0945a4cda
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 0%

---

# 在选项卡之间导航

您可以通过单击各个选项卡或使用表单上的上一个和下一个按钮在选项卡之间导航。
要使用按钮进行导航，请在表单中添加两个按钮，并将其命名为“上一步”和“下一步”。 将以下自定义函数与按钮的单击事件关联，以便在选项卡之间导航。

以下是要在选项卡之间导航的自定义函数。



```javascript
/**
 * Navigate in panel with focusOption
 * @name navigateInPanelWithFocusOption
 * @param {object} panelField
 * @param {string} focusOption - values can be 'nextItem'/'previousItem'
 * @param {scope} globals
 */
function navigateInPanelWithFocusOption(panelField, focusOption, globals)
{
    globals.functions.setFocus(panelField, focusOption);
}
```

下面是“下一个”和“上一个”按钮的规则编辑器

**下一个按钮**

![下一个按钮](assets/next-button.png)

**上一按钮**

![上一按钮](assets/prev-button.png)
