---
title: 级联下拉列表
description: 根据以前的下拉列表选择填充下拉列表。
feature: Adaptive Forms
version: 6.4,6.5
kt: 9724
topic: Development
role: Developer
level: Intermediate
exl-id: f1f2cacc-9ec4-46d6-a6af-dac3f663de78
last-substantial-update: 2021-02-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 0%

---

# 级联下拉列表

级联下拉列表是一系列从属的DropDownList控件，其中一个DropDownList控件取决于父或前一个DropDownList控件。 DropDownList控件中的项目是根据用户从另一个DropDownList控件中选择的项目填充的。

## 用例演示

>[!VIDEO](https://video.tv.adobe.com/v/340344?quality=12&learn=on)

在本教程中，我使用了 [Geonames REST API](http://api.geonames.org/) 来演示此功能。
有许多组织提供此类服务，只要他们有详实的REST API文档，您就可以使用数据集成功能轻松与AEM Forms集成

按照以下步骤在AEM Forms中实施级联下拉列表

## 创建开发人员帐户

使用创建开发人员帐户 [盖纳梅斯](https://www.geonames.org/login). 记下用户名。 需要此用户名才能调用geonames.org的REST API。

## 创建Swagger/OpenAPI文件

OpenAPI规范（以前称为Swagger规范）是REST API的API描述格式。 OpenAPI文件允许您描述整个API，包括：

* 每个端点上的可用端点(/users)和操作(GET/用户、POST/用户)
* 操作参数每个操作的输入和输出身份验证方法
* 联系信息、许可证、使用条款和其他信息。
* API规范可以使用YAML或JSON编写。 该格式易于学习，对人类和机器都易读。

要创建您的第一个swagger/OpenAPI文件，请按照 [OpenAPI文档](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Forms支持OpenAPI规范版本2.0(FKA Swagger)。

使用 [swagger编辑器](https://editor.swagger.io/) 创建swagger文件，以描述获取国家/地区或州的所有国家/地区和子元素的操作。 可以以JSON或YAML格式创建swagger文件。 已完成的swagger文件可从 [此处](assets/swagger-files.zip)
swagger文件描述以下REST API
* [获取所有国家/地区](http://api.geonames.org/countryInfoJSON?username=yourusername)
* [获取Geoname对象的子项](http://api.geonames.org/childrenJSON?formatted=true&amp;geonameId=6252001&amp;username=yourusername)

## 创建数据源

要将AEM/AEM Forms与第三方应用程序集成，我们需要 [创建数据源](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) 云服务配置中的。 请使用 [swagger文件](assets/swagger-files.zip) 创建数据源。
您需要创建2个数据源（一个用于获取所有国家/地区，另一个用于获取子元素）


## 创建表单数据模型

AEM Forms数据集成提供了直观的用户界面，用于创建和使用 [表单数据模型](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). 将表单数据模型基于在前面步骤中创建的数据源。 具有2个数据源的表单数据模型

![fdm](assets/geonames-fdm.png)


## 创建自适应表单

将表单GET模型的调用与自适应表单相集成，以填充下拉列表。
创建一个包含2个下拉列表的自适应表单。 一个用于列出国家/地区，另一个用于根据所选国家/地区列出州/省。

### 填充国家/地区下拉列表

首次初始化表单时，将填充国家/地区列表。 以下屏幕快照显示了配置为填充国家/地区下拉列表选项的规则编辑器。 您必须向用户名提供geonames帐户，才能使其正常工作。
![get-countries](assets/get-countries-rule-editor.png)

#### 填充州/省下拉列表

我们需要根据所选国家/地区填充州/省下拉列表。 以下屏幕截图显示了规则编辑器配置
![state-province-options](assets/state-province-options.png)

### 练习

在表格中添加2个称为县和市的下拉列表，以根据所选国家/地区和省/自治区列出县和市。
![锻炼](assets/cascading-drop-down-exercise.png)
