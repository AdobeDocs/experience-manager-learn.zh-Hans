---
title: 创建自适应表单时要遵循的命名惯例和最佳实践
seo-title: 创建自适应表单时要遵循的命名惯例和最佳实践
description: 创建自适应表单时要遵循的命名惯例和最佳实践
seo-description: 创建自适应表单时要遵循的命名惯例和最佳实践
feature: adaptive-forms
topics: best-practices
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 5b05dbe45babfcfcfc81995d9d48bc9b26b9b6c8
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 0%

---

# 最佳实践

Adobe Experience Manager(AEM)表单可以帮助您将复杂的交易转变为简单、愉悦的数字体验。 以下文档描述了开发自适应Forms时需要遵循的一些其他最佳做法。 此文档应与此文档结 [合使用](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview)

## 命名约定

* **面板**
   * 面板名称以大写字符开头，大小写为驼峰。

* **表单字段**
   * 字段名称以小写字符开头，是大写混合。
   * 不要将字段名称开始为数字
   * 请勿在您的姓名中包含虚线“-”。 这等同于代码中的减号，并在代码中充当运算符。
   * 名称可以包含字母、数字、下划线和美元符号。
   * 名称必须以字母开头
   * 名称区分大小写
   * 保留字（如JavaScript关键字）不能用作名称。 注意其他特定于AF的保留字，如“panel”、“name”。
   * 不要在您的姓名中包含虚线“-”
* **发展Forms**
   * 在开发大型表单时，应考虑表单片段。 启用表单片段的延迟加载以缩短加载时间
   * **数据模型**
      * 建议将自适应表单与相应的数据模型关联
   * **对象事件**
      * 与对象的可见性相关的代码应始终放置在该对象的可见性事件中。
   * **脚本**
      * 如果您在自适应表单中编写的代码超过5个可见行，则必须将代码移至客户端库。 最好将函数添加到客户端库，然后添加相应的jsdoc标签，使函数在自适应表单规则编辑器中可见。


