---
title: 创建自适应表单时要遵循的命名惯例和最佳实践
seo-title: 创建自适应表单时要遵循的命名惯例和最佳实践
description: 创建自适应表单时要遵循的命名惯例和最佳实践
seo-description: 创建自适应表单时要遵循的命名惯例和最佳实践
feature: 自适应表单
topics: best-practices
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '312'
ht-degree: 0%

---

# 最佳实践

Adobe Experience Manager(AEM)表单可以帮助您将复杂的事务转化为简单愉悦的数字体验。 以下文档介绍了开发自适应Forms时需要遵循的一些其他最佳做法。 此文档与[此文档](https://helpx.adobe.com/experience-manager/6-3/forms/using/adaptive-forms-best-practices.html#Overview)一起使用

## 命名约定

* **面板**
   * 面板名称是大小写混合，以大写字符开头。

* **表单域**
   * 字段名称以小写字符开头，以大小写混合。
   * 不要开始带数字的字段名称
   * 请勿在您的姓名中包含短划线“ — ”。 这些值将等同于代码中的减号，并将在代码中充当运算符。
   * 名称可以包含字母、数字、下划线和美元符号。
   * 名称必须以字母开头
   * 名称区分大小写
   * 保留字（如JavaScript关键字）不能用作名称。 注意其他特定于AF的保留字，例如   “panel”、“name”。
   * 不要在您的姓名中包含虚线“ — ”
* **开发Forms**
   * 在开发大型表单时，应考虑表单片段。 启用表单片段的延迟加载以加快加载   times
   * **数据模型**
      * 建议将自适应表单与相应的数据模型关联
   * **对象事件**
      * 应始终将与对象的可见性相关的代码放置在该对象的可见性事件中。
   * **脚本**
      * 如果您在自适应表单中编写的代码超过5个可见行，则必须将代码移到客户端库。 最好将您的函数添加到客户端库，然后添加相应的jsdoc标签，以使函数在自适应表单规则编辑器中可见。


