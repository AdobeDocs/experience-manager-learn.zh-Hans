---
title: 创建自适应表单时应遵循的命名约定和最佳实践
description: 创建自适应表单时应遵循的命名约定和最佳实践
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: fbfc74d7-ba7c-495a-9e3b-63166a3025ab
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 1%

---

# 最佳实践

Adobe Experience Manager(AEM)表单可帮助您将复杂的事务转换为简单、令人愉快的数字体验。 以下文档介绍了开发自适应Forms时需要遵循的一些其他最佳实践。 本文档旨在与 [本文档](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview)

## 命名约定

* **面板**
   * 面板名称以大写字符开头，以驼峰式大小写。

* **表单字段**
   * 字段名称以小写字符开头，采用驼峰式大小写。
   * 请勿以数字开头的字段名称
   * 请勿在您的名称中包含短划线“ — ”。 这些值等同于代码中的减号，并将在代码中充当运算符。
   * 名称可以包含字母、数字、下划线和美元符号。
   * 名称必须以字母开头
   * 名称区分大小写
   * 保留词（如JavaScript关键字）不能用作名称。 请小心其他特定于AF的保留词，如“panel”、“name”。
   * 请勿在您的名称中包含短划线“ — ”
* **开发Forms**
   * 开发大型表单时应考虑表单片段。 启用表单片段的延迟加载，以缩短加载时间
   * **数据模型**
      * 建议将自适应表单与相应的数据模型关联
   * **对象事件**
      * 与对象的可见性相关的代码应始终放置在对象的可见性事件中。
   * **脚本**
      * 如果您在自适应表单中编写的代码超过5行可见，则必须将代码移动到客户端库。 理想情况下，将函数添加到客户端库，然后添加相应的jsdoc标记，以便该函数在自适应表单规则编辑器中可见。
