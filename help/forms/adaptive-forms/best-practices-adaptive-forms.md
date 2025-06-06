---
title: 创建自适应表单时要遵循的命名惯例和最佳实践
description: 创建自适应表单时要遵循的命名惯例和最佳实践
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: fbfc74d7-ba7c-495a-9e3b-63166a3025ab
last-substantial-update: 2020-09-10T00:00:00Z
duration: 57
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 1%

---

# 最佳实践

Adobe Experience Manager (AEM)表单可帮助您将复杂的交易转换为简单、愉快的数字体验。 以下文档介绍了在开发自适应Forms时需要遵循的一些其他最佳实践。 此文档应与[此文档](https://helpx.adobe.com/cn/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview)一起使用

## 命名约定

* **面板**
   * 面板名称是以大写字符开头的驼峰式大小写。

* **表单字段**
   * 字段名称是以小写字符开头的驼峰式大小写。
   * 字段名称不能以数字开头
   * 请勿在您的名称中包含破折号“ — ”。 这些符号将等于代码中的减号，并将充当代码中的运算符。
   * 名称可以包含字母、数字、下划线和美元符号。
   * 名称必须以字母开头
   * 名称区分大小写
   * 不能将保留字(如JavaScript关键字)用作名称。 请注意其他特定于AF的保留字，例如   作为“panel”、“name”。
   * 请勿在您的名称中包含破折号“ — ”
* **正在开发Forms**
   * 开发大型表单时应考虑表单片段。 启用表单片段的延迟加载以加快加载   时间
   * **数据模型**
      * 建议将自适应表单与适当的数据模型关联

   * **对象事件**
      * 与对象的可见性相关的代码应始终置于所述对象的可见性事件中。
   * **脚本**
      * 如果您在自适应表单中编写的代码超出了5行可见，则必须将代码移至客户端库。 理想情况下，将您的函数添加到客户端库，然后添加相应的jsdoc标记，以使该函数在自适应表单规则编辑器中可见。
