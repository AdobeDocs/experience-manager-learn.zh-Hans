---
title: 创建用于列出表单数据的组件
description: 有关创建用于在提交之前审核表单数据的摘要组件的教程。
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2023-01-22T00:00:00Z
exl-id: d537a80a-de61-4d43-bdef-f7d661c43dc8
duration: 33
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 1%

---

# 创建用于汇总表单数据的组件

创建了一个简单组件以列出表单数据以供审阅。 [Guidebridge API的访问函数](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html?q=visit)用于循环访问表单字段。 客户端库中与此组件关联的代码将获取表单上的面板/表组件。 从此面板/表组件的子元素中，表单字段标题、值和SOM表达式是使用GuidBridge API方法提取的。 然后，使用标题、值和SOM表达式构建简单的HTML表，以供最终用户在提交表单之前查看/编辑表单数据。

例如，下面的屏幕截图显示了为列出&#x200B;**YourDetails**&#x200B;的字段及其值而创建的表。 TR中的最后一个TD用于使用字段SOM表达式编辑字段的值。

![visit-func](assets/visit-function.png)

## 后续步骤

[在本地系统上测试解决方案](./deploy-on-your-system.md)
