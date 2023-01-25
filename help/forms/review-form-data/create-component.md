---
title: 创建组件以列出表单数据
description: 有关创建摘要组件以在提交之前审核表单数据的教程。
feature: Adaptive Forms
topics: development
doc-type: tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2023-01-22T00:00:00Z
source-git-commit: d3531e76d3341e0964e5ed878fc72037024a11fd
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---

# 创建组件以汇总表单数据

创建了一个简单组件以列出表单数据以供审阅。 的 [准则桥API的访问函数](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html?q=visit) 用于遍历表单字段。 与此组件关联的客户端库中的代码将获取表单上的面板/表组件。 从此面板/表组件的子元素中，使用GuidBridge API方法提取表单字段标题、值和SOM表达式。 然后，使用标题、值和SOM表达式构建一个简单的HTML表，供最终用户在提交表单之前查看/编辑表单数据。

例如，下面的屏幕截图显示了为列出 **YourDetails**. TR中的最后一个TD用于使用字段SOM表达式编辑字段的值。

![visit-func](assets/visit-function.png)

